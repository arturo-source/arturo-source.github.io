---
title: "What is a web server?"
description: "You use it every day, and now you'll learn what it is, from the very basics."
summary: "You use it every day, and now you'll learn what it is, from the very basics."
date: 2025-01-26T18:19:40+01:00
draft: false
tags: ["tiny pills"]
---

A web server is software that implements the HTTP protocol. Typically, it is accessed using a web browser (Chrome, Firefox, etc.). Being a standard, there are multiple implementations of web servers: Apache and Nginx are the most well-known and widely used.

## How to Use Your Own Web Server?

Later, you will see how to create your own, but the most common scenario is that you want to use an existing one. Although Apache and Nginx are the most used, I recommend using Caddy, as the configuration of the former two can be tedious.

Install Caddy; the process will vary depending on your operating system. Now, create a configuration file named `Caddyfile`:

```caddyfile
http://localhost:8080
root * ./my-website
file_server
```

Next, create some static content. Although the previously mentioned servers can execute code (for example PHP), the content that your browser understands is HTML. Create a folder `my-website` next to the Caddyfile, and create the HTML file `./my-website/index.html`:

```html
<!DOCTYPE html>
<html>
    <head>
        <title>My first web server</title>
    </head>
    <body>
        <p>Hello world!</p>
    </body>
</html>
```

Finally, run `caddy start`, and access your browser at <http://localhost:8080/>. You should see "Hello world!" in your browser.

## How Does It Work?

I recommend using `curl` instead of a browser to see what's happening at a low level:

```bash
curl http://localhost:8080 -v
```

The terminal output will look something like this:

```bash
* Host localhost:8080 was resolved.
* IPv6: ::1
* IPv4: 127.0.0.1
*   Trying [::1]:8080...
* Connected to localhost (::1) port 8080
* using HTTP/1.x
> GET / HTTP/1.1
> Host: localhost:8080
> User-Agent: curl/8.11.1
> Accept: */*
> 
* Request completely sent off
```

What you see with the `>` symbol are the bytes that `curl` sends to your server.

```bash
< HTTP/1.1 200 OK
< Accept-Ranges: bytes
< Content-Length: 149
< Content-Type: text/html; charset=utf-8
< Etag: "sqpjdl45"
< Last-Modified: Sun, 26 Jan 2025 18:00:57 GMT
< Server: Caddy
< Date: Sun, 26 Jan 2025 18:14:31 GMT
< 
...here goes your html...
* Connection #0 to host localhost left intact
```

What you see with the `<` symbol are the bytes that the server has responded with.
The HTTP protocol works as follows, as you may have noticed, the request consists of:

1. First line: verb - route - HTTP version (the verb can be GET, POST, PUT, etc.).
2. One header per line (name, colon, value).
3. Double newline.
4. Body of the request (not visible because GET requests do not have a body).
5. Double newline (if you had included a body).

Newlines are important because they indicate the end of a section, but to be sure where the body ends, you must use the `Content-Length` header, as seen in the response.

And the response will look something similar:

1. First line: HTTP version - [code](https://http.cat/) - explanation.
2. One header per line (name, colon, value).
3. Double newline.
4. Body of the response (in this case, it's HTML, but it could be another type of content).
5. Double newline.

## Create Your First Web Server From Scratch

If you understood the previous point, you are ready to create your own web server. This is not necessary in 99% of cases, as you usually want an existing server, but it is an interesting exercise to undertake.

The following is a **very basic** example of how I would program a web server using Go. I only consider the best-case scenario, where the request is correct, but obviously all cases should be considered. I encourage you to do the same in another language and expand upon it to see if you understand the concepts.

```go
package main

import (
 "bufio"
 "fmt"
 "net"
 "strings"
)

func main() {
 listener, err := net.Listen("tcp", ":8080")
 if err != nil {
  fmt.Println("Error starting:", err)
  return
 }
 defer listener.Close()

 fmt.Println("Server listening on http://localhost:8080")

 for {
  conn, err := listener.Accept()
  if err != nil {
   fmt.Println("Error accepting connection:", err)
   continue
  }

  go handleConnection(conn)
 }
}

func handleConnection(conn net.Conn) {
 defer conn.Close()

 reader := bufio.NewReader(conn)
 requestLine, err := reader.ReadString('\n')
 if err != nil {
  fmt.Println("Error reading the request:", err)
  return
 }

 requestParts := strings.Split(strings.TrimSpace(requestLine), " ")
 if len(requestParts) < 3 {
  fmt.Println("Malformed request:", requestLine)
  return
 }
 method, path, httpVersion := requestParts[0], requestParts[1], requestParts[2]
 fmt.Printf("Request received: %s %s %s\n", method, path, httpVersion)

 for {
  line, err := reader.ReadString('\n')
  if err != nil || line == "\r\n" {
   break
  }
 }

 responseBody := "<html><head><title>My first web server</title></head><body><p>Hello world!</p></body></html>"
 response := fmt.Sprintf("%s 200 OK\r\n", httpVersion) +
  "Content-Type: text/html\r\n" +
  fmt.Sprintf("Content-Length: %d\r\n", len(responseBody)) +
  "\r\n" +
  responseBody

 _, err = conn.Write([]byte(response))
 if err != nil {
  fmt.Println("Error sending response:", err)
 }
}
```

To check if it's working, run your program, in this case `go run main.go`, and enter your browser at <http://localhost:8080/>. You should see the same webpage as before. Try using `curl` and compare the differences with Caddy.
