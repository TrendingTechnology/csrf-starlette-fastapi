# csrf-starlette-fastapi
Dead simple CSRF security middleware for Starlette ⭐ and Fast API ⚡

* Will work with either a `<input type="hidden">` field or ajax request headers, interchangeably.
* Uses stateless [Double Submit Cookie](https://cheatsheetseries.owasp.org/cheatsheets/Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.html#double-submit-cookie) method, like Django.
* Tiny, easy to audit.

### Install

Add `csrf_middleware.py` to your project `/middleware` folder.

### Add to Starlette

```py
from starlette.applications import Starlette
from starlette.middleware import Middleware
from middleware.csrf_middleware import CSRFMiddleware

routes = ...

middleware = [
    Middleware(CSRFMiddleware)
]

app = Starlette(routes=routes, middleware=middleware)
```
### Add to FastAPI

```py
from fastapi import FastAPI
from middleware.csrf_middleware import CSRFMiddleware

app = FastAPI()
app.add_middleware(CSRFMiddleware)
```
### Usage
* Use directly in HTML.
  * Pass `request.state.csrftoken` to your [template engine](https://www.starlette.io/templates/).
  * `<input type="hidden" name="csrftoken" value="{{ csrftoken }}" />`
* Use javascript / ajax frameworks such as the elegant [htmx](https://htmx.org/) ♥️
  * Before your ajax call, set your headers.
    * Most frameworks: `headers: { 'csrftoken': '{{ csrftoken }}' }`
    * [XMLHttpRequest.setRequestHeader()](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest/setRequestHeader)
* Another [htmx](https://htmx.org/) ♥️ solution.
  * `<body hx-headers='{"csrftoken": "{{ csrftoken }}"}'>`

### Why?

To make available something more simple and auditable than the typical libraries for this as of 2022:
* https://github.com/simonw/asgi-csrf
* https://github.com/frankie567/starlette-csrf
* https://github.com/piccolo-orm/piccolo_api/blob/master/piccolo_api/csrf/middleware.py 

### Do I need CSRF Middleware?

Maybe? Maybe not?

* https://scotthelme.co.uk/csrf-is-dead/
* https://scotthelme.co.uk/csrf-is-really-dead/
* https://simonwillison.net/2021/Aug/3/samesite/
* Ongoing: https://github.com/encode/starlette/discussions/1411
