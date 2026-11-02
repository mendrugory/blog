---
title: EFK for non-cloud-native Applications in Kubernetes
date: 2018-09-18
url: /post/efk-for-non-cloud-native-apps
---

Recently I worked on a project where I had to migrate a Mesos infrastructure to Kubernetes, and one of the key requirements was to replace **ELK** _(elasticsearch - logstash - kibana)_ with **EFK** _(elasticsearch - fluentd - kibana)_.

You can find a lot of information about how to use EFK in Kubernetes when your applications log to standard output, by putting a Fluentd agent on every node, but that was not the case for all the applications in the Mesos infrastructure. Most of them logged to a specific file and had a custom log rotator.

I could not use Fluentd in the usual way, and I could not modify the applications to change their log configuration, so my approach was to add a Fluentd agent to every pod following the sidecar pattern, and to mount a shared volume so Fluentd could read the application's logs.

All the Fluentd agents behave the same way, so they can share the same configuration through a `ConfigMap`:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: fluentd-config
  namespace: default
data:
  fluentd.conf: |
    <source>
      @type tail
      tag *
      path /fluentd/log/*.log
      pos_file /fluentd/log/logging.log.pos
      <parse>
        @type none
      </parse>
    </source>

    <filter **>
      @type   record_transformer
      <record>
        hostname ${hostname}
        log_file ${tag_suffix[0]}
      </record>
    </filter>


    <match **>
      @type copy
      <store>
        @type             elasticsearch
        host              elasticsearch.logging.svc.cluster.local
        logstash_format   true
        utc_index         true
        time_key          time
      </store>
    </match> 
```


And a typical pod would look like this:

```yaml
apiVersion: apps/v1beta1
kind: Deployment
metadata:
  name: myapp
spec:
  replicas: 1
  template:
    metadata:
      name: myapp
      labels:
        app: myapp
    spec:
      containers:
      - name: myapp
        image: myapp:latest
        ports:
        - containerPort: 8000
          name: web 
        volumeMounts:
        - name: logging
          mountPath: /var/log/myapp/
        readinessProbe:  
          httpGet:
            path: /index
            port: 8000
          initialDelaySeconds: 30
          periodSeconds: 60                    
      - name: fluentd
        image: forkdelta/fluentd-elasticsearch
        env:
        - name: FLUENTD_CONF
          value: fluentd.conf
        volumeMounts:
        - name: logging
          mountPath: /fluentd/log/
        - name: fluentd-config
          mountPath: /fluentd/etc/
      - name: logrotator
        image: blacklabelops/logrotate:1.3
        env:
          - name: LOGS_DIRECTORIES
            value: /var/log/application
          - name: LOGROTATE_INTERVAL
            value: hourly  
          - name: LOGROTATE_SIZE
            value: 100k
        volumeMounts:
        - name: logging
          mountPath: /var/log/application            
      volumes:
      - name: fluentd-config
        configMap:
          name: fluentd-config
      - name: logging
        emptyDir: {}                     
```


Although this approach consumes more of your cluster's resources, especially RAM, at least you can collect your applications' logs without modifying them, and it gives the people responsible (maybe you) time to change the applications' log configuration.


Here you can also find the `yamls` for the logging namespace:

`logging-ns.yaml`

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: logging
```

`es-curator-config.yaml`

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: curator-config
  namespace: logging
data:
  action_file.yml: |-
    ---
    # Remember, leave a key empty if there is no value.  None will be a string,
    # not a Python "NoneType"
    #
    # Also remember that all examples have 'disable_action' set to True.  If you
    # want to use this action as a template, be sure to set this to False after
    # copying it.
    actions:
      1:
        action: delete_indices
        description: "Clean up ES by deleting old indices"
        options:
          timeout_override:
          continue_if_exception: False
          disable_action: False
          ignore_empty_list: True
        filters:
        - filtertype: age
          source: name
          direction: older
          timestring: '%Y.%m.%d'
          unit: days
          unit_count: 7
          field:
          stats_result:
          epoch:
          exclude: False
  config.yml: |-
    ---
    # Remember, leave a key empty if there is no value.  None will be a string,
    # not a Python "NoneType"
    client:
      hosts:
        - elasticsearch.logging.svc.cluster.local
      port: 9200
      url_prefix:
      use_ssl: False
      certificate:
      client_cert:
      client_key:
      ssl_no_validate: False
      http_auth:
      timeout: 30
      master_only: False

    logging:
      loglevel: INFO
      logfile:
      logformat: default
      blacklist: ['elasticsearch', 'urllib3']

```

`es-curator.yaml`

```yaml
apiVersion: batch/
kind: CronJob
metadata:
  name: curator
  namespace: logging
spec:
  schedule: 5 6 * * *
  jobTemplate:
    spec:
      template:
        spec:
          containers:
          - name: curator
            image: bobrik/curator
            args: ["--config", "/etc/config/config.yml", "/etc/config/action_file.yml"]
            volumeMounts:
              - name: config-volume
                mountPath: /etc/config
          volumes:
            - name: config-volume
              configMap:
                name: curator-config
          restartPolicy: OnFailure
```

`elasticsearch.yaml`

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  namespace: logging
  name: elasticsearch
  labels:
    component: elasticsearch
spec:
  replicas: 1
  selector:
    matchLabels:
     component: elasticsearch
  template:
    metadata:
      labels:
        component: elasticsearch
    spec:
      initContainers:
      - name: init-sysctl
        image: busybox:1.27.2
        command:
        - sysctl
        - -w
        - vm.max_map_count=262144
        securityContext:
          privileged: true
      containers:
      - name: elasticsearch
        image: docker.elastic.co/elasticsearch/elasticsearch:6.3.2
        env:
        - name: NAMESPACE
          valueFrom:
            fieldRef:
              fieldPath: metadata.namespace
        - name: HTTP_ENABLE
          value: "false"
        - name: ES_JAVA_OPTS
          value: -Xms512m -Xmx512m
        - name: PROCESSORS
          valueFrom:
            resourceFieldRef:
              resource: limits.cpu
        resources:
          limits:
            cpu: 1
          requests:
            cpu: 100m
        ports:
        - containerPort: 9200
          name: http
---          
apiVersion: v1
kind: Service
metadata:
  name: elasticsearch
  namespace: logging
  labels:
    component: elasticsearch
spec:
  selector:
    component: elasticsearch
  ports:
  - name: http
    port: 9200
    targetPort: http
```


`kibana.yaml`

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  namespace: logging
  name: kibana
  labels:
    component: kibana
spec:
  replicas: 1
  selector:
    matchLabels:
     component: kibana
  template:
    metadata:
      labels:
        component: kibana
    spec:
      containers:
      - name: kibana
        image: docker.elastic.co/kibana/kibana-oss:6.2.2
        env:
        - name: ELASTICSEARCH_URL
          value: http://elasticsearch.logging.svc.cluster.local:9200
        resources:
          limits:
            cpu: 1000m
          requests:
            cpu: 100m
        ports:
        - containerPort: 5601
          name: http
---          
apiVersion: v1
kind: Service
metadata:
  name: kibana
  namespace: logging
  labels:
    component: kibana
spec:
  selector:
    component: kibana
  ports:
  - name: http
    port: 5601
    targetPort: http
```


