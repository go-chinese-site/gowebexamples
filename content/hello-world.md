+++
weight = 1
title = "Hello World"
description = "This examples shows how to create an HTTP server using the net/http package from the standard library. It contains all functionalities about the HTTP protocol."
+++

# Hello World

## Introduction
Go 是一个自带干粮的编程语言，拥有自己的内置webserver.标准库中的net/http 包包含了关于HTTP协议的全部功能的函数。包中包含了一个HTTP client和 一个HTTP server . 在本例子中你将会发现成功创建一个你可以在浏览器中浏览网页的webserver是有多么简单。

{{< edison >}}

## Registering a Request Handler
首先，创建一个从浏览器接受进来的HTTP链接、HTTP client 或者API请求的Handler， 在Go 中是一个函数，它在创建的时候，形态上有如下相似特点:{{< highlight go >}} func (w http.ResponseWriter, r *http.Request) {{< / highlight >}}

这个函数接受两个参数:

1. `http.ResponseWriter`: 这个参数是给你写你的text/html响应的
2. `http.Request` : 它包含所有HTTP请求的信息，比如URL 或者 header

注册一个的HTTP Server 的request handler 非常简单，如下就是:
{{< highlight go >}}
http.HandleFunc("/", func (w http.ResponseWriter, r *http.Request) {
	fmt.Fprintf(w, "Hello, you've requested: %s\n", r.URL.Path)
})
{{< / highlight >}}

## Listen for HTTP Connections
一个单独的request handler 不能接受任何外部的HTTP 链接。一个HTTP Server, 为了把链接发送给request handle， 它必须监听一个端口。由于80端口是大多数默认的HTTP 流量通道， 所以本server 也会用80端口来监控。下面的代码会启动一个GO的默认HTTP server ,同时监听80端口上的链接。你可以在你的浏览器中访问 http://localhost/, 同时可以看到你的server 正在处理你的request
{{< highlight go >}}
http.ListenAndServe(":80", nil)
{{< / highlight >}}

## The Code (for copy/paste)
下面是完整的代码.
{{< highlight go >}}
package main

import (
	"fmt"
	"net/http"
)

func main() {
	http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
		fmt.Fprintf(w, "Hello, you've requested: %s\n", r.URL.Path)
	})

	http.ListenAndServe(":80", nil)
}

{{< / highlight >}}