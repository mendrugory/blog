---
title: Mesos for Local Development
date: 2018-12-18
url: /post/python3-7-asyncio-chat
---

Python is one of the most used languages because the ramp up is really low. A good example is the API of [ayncio](https://docs.python.org/3/library/asyncio-stream.html) for building [I/O bound applications](https://en.wikipedia.org/wiki/I/O_bound).

Creating asynchronous code could be really tough, but using asyncio could be a *child's play*.

We are going to try to build a simple chat server in this post using **Python 3.7**. As the server will be a TCP server, the clients only need to use [Telnet](https://es.wikipedia.org/wiki/Telnet) to interact with.


## Echo TCP Server

The first version is an *Echo TCP Server* where the client will receive its messages until it writes *"exit"*.

```python
import asyncio

async def handle(reader, writer):
    addr = writer.get_extra_info('peername')
    message = f"{addr!r} is connected !!!!"
    print(message)
    while True:
        data = await reader.read(100)
        message = data.decode().strip()
        writer.write(data)
        await writer.drain()
        if message == "exit":
            message = f"{addr!r} wants to close the connection."
            print(message)
            break
    writer.close()

async def main():
    server = await asyncio.start_server(
        handle, '127.0.0.1', 8888)
    addr = server.sockets[0].getsockname()
    print(f'Serving on {addr}')
    async with server:
        await server.serve_forever()

asyncio.run(main())
```

Open a console and type:

```bash
$ telnet 127.0.0.1 8888
Trying 127.0.0.1...
Connected to 127.0.0.1.
Escape character is '^]'.
```

But talking with your self is not always fun, so, let's improve it.



## Chat Server

This server will forward the messages of one client to all the other connected clients.

```python
import asyncio

writers = []

def forward(writer, addr, message):
    for w in writers:
        if w != writer:
            w.write(f"{addr!r}: {message!r}\n".encode())

async def handle(reader, writer):
    writers.append(writer)
    addr = writer.get_extra_info('peername')
    message = f"{addr!r} is connected !!!!"
    print(message)
    forward(writer, addr, message)
    while True:
        data = await reader.read(100)
        message = data.decode().strip()
        forward(writer, addr, message)
        await writer.drain()
        if message == "exit":
            message = f"{addr!r} wants to close the connection."
            print(message)
            forward(writer, "Server", message)
            break
    writers.remove(writer)
    writer.close()

async def main():
    server = await asyncio.start_server(
        handle, '127.0.0.1', 8888)
    addr = server.sockets[0].getsockname()
    print(f'Serving on {addr}')
    async with server:
        await server.serve_forever()

asyncio.run(main())
```


Open several consoles and type:

```bash
$ telnet 127.0.0.1 8888
Trying 127.0.0.1...
Connected to 127.0.0.1.
Escape character is '^]'.
```

This one could be considered an actual chat server.

## Proposal Bonus Track

One of the downsides of Python is the [GIL](https://realpython.com/python-gil/) which limits the number of paralell threads to only one.

This limitation makes really tough to scale up vertically, especially if your application has state.

If we want to scale up our chat server (vertical or horizontal), without using other tools than Python, a possible solution would be to add a new TCP server to the *event loop* which would be used to communicate between chat servers.

## Conclusion

Building a simple script which works as chat server using Python3.7 and its library asyncio is a good exercise.

Enjoy !!