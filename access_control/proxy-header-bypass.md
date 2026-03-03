VULNERABILITY:
->Access control is enforced at the reverse proxy layer, but not validated by the backend application.The backend framework supports non-standard headers (e.g., X-Original-URL,X-Rewrite-URL) that can rewrite the request path after proxy filtering.

example proxy rule: DENY POST /admin/deleteUser , non-managers


DEVELOPER ASSUMPTION:
->backend server application framework assumes comming request to /admin or /admin/deleteUser path has already been enforced authroization check by the proxy server,Therefore the backend does not perform its own validation


Why this is dangerous:
->if the backend framework internally honours internal rewrite path headers such as:
    1)X-Original-URL
    2)X-Rewrite-URL
    etc

An attacker can:
    send a harmless path (/)
    add a rewrite header:X-Original-URL:/admin
    cause the backend to internally process /admin

EXPLOITATION STEPS:
1)Intercept the GET request being made to the / path.
2)chnage the path to /admin
3)analyse the reponse headers,length,content type mismatch comapred to other api responses.THis tells whether authorization is perfomed and path restricted by proxy or server.
4)Add new header: X-Original-URL: /admin
5)Check the response.
    200 ok->successfully bypassed restriction
    403 forbidden -> either the proxy restricts use of non standatd http methods or the server is performing auth check.

Real-World Testing Pattern:
->Test if access control is enforced at proxy or backend.
->Check for rewrite-related headers (X-Original-URL, X-Rewrite-URL).
->Compare direct backend port access if exposed.
->Look for inconsistent authorization between routes and subroutes.