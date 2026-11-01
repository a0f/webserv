# webserv

An HTTP/1.1 web server in C++20, built at Codam (42 network) with [DjaxBank](https://github.com/DjaxBank). The original repository is [DjaxBank/webserv](https://github.com/DjaxBank/webserv).

## What it does

- Serves static sites from an nginx-style config file: ports, routes, allowed methods, redirects, default pages, directory listings and error pages
- Handles `GET`, `POST` and `DELETE`, including chunked request bodies
- Runs CGI scripts (for example PHP) and returns their output
- Cookies and sessions
- One process, non-blocking I/O multiplexing across all client connections
- Tested under load with `siege`

## Build and run

```sh
make
./webserv example.conf
```

Then open the address and port set in `example.conf` in a browser.

## How it works

1. **Sockets:** the server listens on every port in the config and watches all connections in a single event loop.
2. **Request parsing:** each request is read incrementally and parsed into its request line, headers and body, rejecting malformed input with the correct status code.
3. **Routing and response:** the request is matched to a route, then served as a file, a directory listing, an upload or a CGI result.

## Who did what

- **Me:** the HTTP request parser: request line, headers, URI parsing and normalisation, chunked and fixed-length bodies, with its own tests and debug tool. Also parts of the server loop and responses.
- **DjaxBank:** the response builder, client handling, CGI, the config parser and sockets.
