# asgitools

A collection of tools for developing [ASGI] applications. Supports both ASGI
 servers, uvicorn and daphne.

**Requirements**:

- Python 3.5.3+
- ASGI server, [uvicorn] or [daphne]

## Quickstart

You can find a very simple http/websocket example in `examples/` that demonstrates both protocol and url routing. Otherwise you can try the example below to run a simple HTTP app.

Install using `pip`:

- TODO

Create an application, in `app.py`:

```python
from asgitools.routing import (
    AsgiProtocolRouter,
    AsgiProtocol,
    AsgiUrlRouter,
    AsgiUrlRoute
)


class HttpConsumer:

    def __init__(self, scope):
        self.scope = scope

    async def __call__(self, receive, send):
        message = await receive()
        if message['type'] == 'http.request':
            await send({
                'type': 'http.response.start',
                'status': 200,
                'headers': [
                    [b'content-type', b'text/html'],
                ],
            })
            await send({
                'type': 'http.response.body',
                'body': b'Hello world!',
                'more_body': False,
            })


app = AsgiProtocolRouter([
    AsgiProtocol(
        'http',
        AsgiUrlRouter([
            AsgiUrlRoute(
                '/', HttpConsumer, methods=['GET']
            ),
        ])
    ),
])

```

Run the server:

```shell
$ uvicorn app:App
```

OR

```shell
$ daphne app:app
```
---

# Todo

- Tests
- Middleware
- Better examples

[ASGI]: https://github.com/django/asgiref/blob/master/specs/asgi.rst
[uvicorn]: https://github.com/encode/uvicorn
[daphne]: https://github.com/django/daphne
