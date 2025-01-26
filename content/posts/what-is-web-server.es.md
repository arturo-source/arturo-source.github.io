---
title: "¿Qué es un servidor web?"
slug: "que-es-servidor-web"
description: "Lo usas a diario, y ahora aprenderás qué es, desde lo más básico."
summary: "Lo usas a diario, y ahora aprenderás qué es, desde lo más básico."
date: 2025-01-26T18:19:15+01:00
draft: false
tags: ["mini pildoras"]
---

Un servidor web es un software que implementa el protocolo HTTP. Normalmente se accede usando un navegador web (Chrome, Firefox, etc.). Al ser un estándar, existen múltiples implementaciones de servidores web: Apache y Nginx son los más conocidos y usados.

## ¿Cómo usar tu propio servidor web?

Más adelante verás cómo crear uno propio, pero lo más común es que quieras usar uno ya existente. Aunque Apache y Nginx sean los más usados, te recomiendo usar Caddy, puesto que la configuración de los dos anteriores puede llegar a ser tediosa.

Instala Caddy, en cada sistema operativo será distinto. Y ahora crea un archivo de configuración llamado `Caddyfile`:

```caddyfile
http://localhost:8080

root * ./my-website
file_server
```

Lo siguiente será crear algo de contenido estático, aunque los servidores anteriormente mencionados pueden ejecutar código (por ejemplo PHP), el contenido que entenderá tu navegador es HTML. Crea una carpeta `my-website` al lado del Caddyfile, y crea el html `./my-website/index.html`:

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

Por último, ejecuta `caddy start`, y accede en tu navegador a <http://localhost:8080/> , deberías ver "Hello world!" en tu navegador.

## ¿Cómo funciona?

Te recomiendo usar `curl`, en lugar del navegador, para ver qué está pasando a bajo nivel:

```bash
curl http://localhost:8080 -v
```

La salida de la terminal será algo parecido a esto:

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

Lo que ves con el símbolo `>`  son los bytes que envía curl a tu servidor.

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

Lo que ves con el símbolo `<` son bytes que te ha respondido el servidor.

El protocolo HTTP funciona de la siguiente manera, como habrás apreciado la request se compone de:

1. Primera linea: verbo - ruta - version HTTP (el verbo puede ser GET, POST, PUT...).

2. Una cabecera por cada linea (nombre, doble punto, valor).

3. Doble salto de linea.

4. Cuerpo de la request (no se ve porque las peticiones GET no tienen cuerpo).

5. Doble salto de linea (en caso de que hayas puesto un cuerpo).

Los saltos de linea son importantes porque indican la finalización de una sección, pero para estar seguro dónde termina el cuerpo, deberás usar la cabecera `Content-Length`, como puedes ver en la response.

Y la response será algo paredido:

1. Primera linea: versión HTTP - [código](https://http.cat/) - explicación.

2. Una cabecera por cada linea (nombre, doble punto, valor).

3. Doble salto de linea.

4. Cuerpo de la response (en este caso es html, pero podría ser otro contenido).

5. Doble salto de linea.

## Crea tu primer servidor web desde cero

Si has entendido el anterior punto, ya estás preparado para crear tu propio servidor web. No es algo que sea necesario, en el 99% de los casos quieres un servidor ya existente, pero es un ejercicio interesante de hacer.

El siguiente es un ejemplo **muy básico** de cómo programaría un servidor web usando Go. Solo contemplo el mejor caso, en el que la solicitud es correcta, pero obviamente habría que contemplarlos todos. Te animo a que hagas lo mismo en otro lenguaje, y lo amplies, para ver si has entendido los conceptos.

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

Para ver si está funcionando, ejecuta tu programa, en este caso `go run main.go`, y entra en tu navegador en <http://localhost:8080/> , deberías ver la misma web que antes. Prueba a usar curl y ver las diferencias con caddy.
