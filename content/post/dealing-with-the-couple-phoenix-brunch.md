---
title: "Dealing with the couple Phoenix & Brunch"
date: 2017-02-11
url: /post/dealing-with-the-couple-phoenix-brunch
index: true
---

<p style="text-align:center">
  <img alt="Phoenix" src="/img/phoenix_1.png" style="max-width:200px;width:100%"/>
</p>

I am currently working on a project where I have to integrate many real-time data sources and show the results to the users as soon as possible. As you can imagine, [Elixir](http://elixir-lang.org/) + [PhoenixFramework](http://phoenixframework.org/) are my tools of choice. This kind of project always brings a lot of challenges that we have to solve (check out my fork of [Floki](https://github.com/mendrugory/floki)). But the most tedious one has not been directly related to the particulars of this project: it has been dealing with [Brunch](http://brunch.io/). Brunch is the front-end build tool that Phoenix ships with by default. It seems simple (especially compared with [Webpack](https://webpack.github.io/)), but my problems began when I needed to execute different JavaScript scripts depending on the rendered Phoenix template.

<p style="text-align:center">
  <img alt="Brunch" src="/img/brunch_1.jpeg" style="max-width:300px;width:100%"/>
</p>

Phoenix renders the HTML on the server side, and Brunch compiles and joins everything (by default) into one JavaScript file (or maybe two, if you want to keep the 3rd-party code separate). As a result, you have a single entry point. But if you have several "views" rendered by the server, you will probably have different needs covered by different pieces of JavaScript code, and you don't want to always execute all of them.

**How did I finally solve it?**

Brunch can compile and join the JavaScript files however you decide, so I created two main folders inside the web/static/js path:

- common: it will contain all the logic
- specific: it will contain the entry points

We tell Brunch to produce a common.js file from all the JavaScript files that are not in the "specific" folder (common + 3rd party), and one file per template under the "specific" folder. (For my own sanity, I created one folder per template, where the js file and the folder have the same name. In the future you may need more specific logic in each template, with more js files in each folder.) I know that we will end up producing more files than we would have liked, but the heaviest part will be in common.js, and each entry-point JavaScript file will contain only a few lines of code (in my case, fewer than 10 lines each), because we only use these files for initialization.

```javascript
exports.config = {
  files: {
    javascripts: {
      joinTo: {
        "js/common.js": /(?!web\/static\/js\/specific)/,
        "js/myscript.js": /^(web\/static\/js\/specific\/one)/,
        "js/myscript2.js": /^(web\/static\/js\/specific\/two)/,
        "js/myscript3.js": /^(web\/static\/js\/specific\/three)/
      }
...
}
```

All the Phoenix templates are rendered using a layout (web/static/templates/layout/app.html.eex). This file assumes that Brunch is going to produce a JavaScript file called app.js, which contains all the logic we need as well as the entry point. In our case, we have to replace this file with common.js, because this new file will contain all the logic. Then we have to add two new lines:

```html
<script type="text/javascript" src="<%= static_path(@conn, "/your_app/js/common.js") %>"></script>
<script type="text/javascript" src="<%= static_path(@conn, "/your_app/js/" <> assigns[:js_script]) <> ".js" %>"></script>
<script>require("<%= "web/static/js/specific/" <> assigns[:js_script] <> "/" <> assigns[:js_script] %>")</script>
```

The first one loads the JavaScript file needed in each case, and the second one marks it as the entry point, so it will be executed.

As you can see, we use the ability to read custom values stored in the connection, so we only have to set the `:js_script` variable to indicate which script should be used in each case.

```elixir
defmodule YourApp.YourController do
  use YourApp.Web, :controller

  def index(conn, _params) do
    conn
    |> assign(:js_script, "my_script")
    |> render("index.html")
  end

end
```

The main logic and the 3rd-party code, which are the heaviest parts, will be in common.js, so they will be downloaded once and cached by the browser. The specific parts, just your few lines of initialization, will be downloaded for each template, although they will end up cached as well.

This was the simplest method I found to solve my problem. I hope it is useful for someone else.
