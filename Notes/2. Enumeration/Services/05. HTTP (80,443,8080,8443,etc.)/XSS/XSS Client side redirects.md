# XSS Client side redirects

While pentesting webapps, whenever you notice a redirect, check what caused it.

If it's a client side redirect (caused by JavaSCript), try redirecting to javascript:alert(), now you have XSS!

Or even better, if you go to /admin (or similar) and it's a client side redirect then you have improper access controls. 
