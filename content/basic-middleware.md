+++
weight = 5
title = "Middleware (Basic)"
description = "This example will show how to create basic logging middleware in the Go programming language."
+++

# Middleware (Basic)
这个例子将展示在Go中创建基本的日志中间件.


这个例子将展示如何在GO中创建基本的日志中间件。一个中间件只需要一个`http.HandlerFunc` 作为它其中的一个参数，装饰它，并返回一个新的`http.HandlerFunc`用于服务器调用

{{< edison >}}

{{< highlight go >}}
// basic-middleware.go
package main

import (
	"fmt"
	"log"
	"net/http"
)

func logging(f http.HandlerFunc) http.HandlerFunc {
	return func(w http.ResponseWriter, r *http.Request) {
		log.Println(r.URL.Path)
		f(w, r)
	}
}

func foo(w http.ResponseWriter, r *http.Request) {
	fmt.Fprintln(w, "foo")
}

func bar(w http.ResponseWriter, r *http.Request) {
	fmt.Fprintln(w, "bar")
}

func main() {
	http.HandleFunc("/foo", logging(foo))
	http.HandleFunc("/bar", logging(bar))

	http.ListenAndServe(":8080", nil)
}
{{< / highlight >}}
{{< highlight console >}}
$ go run basic-middleware.go
2017/02/10 23:59:34 /foo
2017/02/10 23:59:35 /bar
2017/02/10 23:59:36 /foo?bar

$ curl -s http://localhost:8080/foo
$ curl -s http://localhost:8080/bar
$ curl -s http://localhost:8080/foo?bar
{{< / highlight >}}
