+++
weight = 6
title = "Middleware (Advanced)"
description = "This example will show how to create a more advanced version of middleware in the Go programming language."
+++

# Middleware (Advanced)

这个例子将展示如何在GO中创建更高级的中间件。一个中间件只需要一个`http.HandlerFunc` 作为它的一个参数，装饰它并把它作为一个新的 `http.HandlerFunc`  给服务器调用.

在这里，我们定义一个新的类型`Middleware`, 它可以更容易的链接多个中间件。这个想法灵感来自于Mat Ryer 谈论如果建立APIs.
你可以从 <a target="_blank" href="https://medium.com/@matryer/writing-middleware-in-golang-and-how-go-makes-it-so-much-fun-4375c1246e81">这里</a>查看更详细的内容.

{{< edison >}}

<br />
这个片段详细展示了如何创建一个新的中间件。此处下面是完整的例子，这里我们减少了一些样板代码.
{{< highlight go >}}
func createNewMiddleware() Middleware {

	// Create a new Middleware
	middleware := func(next http.HandlerFunc) http.HandlerFunc {

		// Define the http.HandlerFunc which is called by the server eventually
		handler := func(w http.ResponseWriter, r *http.Request) {

			// ... do middleware things

			// Call the next middleware/handler in chain
			next(w, r)
		}

		// Return newly created handler
		return handler
	}

	// Return newly created middleware
	return middleware
}
{{< / highlight >}}
<br />
下面是完整的例子:
{{< highlight go >}}
// advanced-middleware.go
package main

import (
	"fmt"
	"log"
	"net/http"
	"time"
)

type Middleware func(http.HandlerFunc) http.HandlerFunc

// Logging logs all requests with its path and the time it took to process
func Logging() Middleware {

	// Create a new Middleware
	return func(f http.HandlerFunc) http.HandlerFunc {

		// Define the http.HandlerFunc
		return func(w http.ResponseWriter, r *http.Request) {

			// Do middleware things
			start := time.Now()
			defer func() { log.Println(r.URL.Path, time.Since(start)) }()

			// Call the next middleware/handler in chain
			f(w, r)
		}
	}
}

// Method ensures that url can only be requested with a specific method, else returns a 400 Bad Request
func Method(m string) Middleware {

	// Create a new Middleware
	return func(f http.HandlerFunc) http.HandlerFunc {

		// Define the http.HandlerFunc
		return func(w http.ResponseWriter, r *http.Request) {

			// Do middleware things
			if r.Method != m {
				http.Error(w, http.StatusText(http.StatusBadRequest), http.StatusBadRequest)
				return
			}

			// Call the next middleware/handler in chain
			f(w, r)
		}
	}
}

// Chain applies middlewares to a http.HandlerFunc
func Chain(f http.HandlerFunc, middlewares ...Middleware) http.HandlerFunc {
	for _, m := range middlewares {
		f = m(f)
	}
	return f
}

func Hello(w http.ResponseWriter, r *http.Request) {
	fmt.Fprintln(w, "hello world")
}

func main() {
	http.HandleFunc("/", Chain(Hello, Method("GET"), Logging()))
	http.ListenAndServe(":8080", nil)
}
{{< / highlight >}}
{{< highlight console >}}
$ go run advanced-middleware.go
2017/02/11 00:34:53 / 0s

$ curl -s http://localhost:8080/
hello world

$ curl -s -XPOST http://localhost:8080/
Bad Request
{{< / highlight >}}


