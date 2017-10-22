+++
weight = 3
title = "Templates"
description = "This example shows how to use the `gorilla/mux` package to create routes with named parameters, GET/POST handlers and domain restrictions."
+++

# Templates

## Introduction
Go 的`html/template`包对HTML模板提供了丰富的模板语言。它主要用于Web应用程序，在客户端的浏览中以结构化的方式显示数据。Go 模板语言最大的好处就是数据的自动转义。没有必要担心在显示浏览器之前，那些作为GO解析HTML模板的XSS攻击和避开所有输入.

{{< edison >}}

## First Template
GO写一个模板非常简单。这个例子展示一个TODO 列表, 用HTML中的无序列表 （ul）来写， 当渲染模板时， 可以从GO的各种数据结构中导入。它可以是简单的string 或 数字， 它甚至可以是嵌套的数据结构。要访问模板中的数据，最前的变量是通过{{. }} 来访问。其中 花括号中的点(.) 称为管道和root.

{{< highlight go >}}
data := TodoPageData{
	PageTitle: "My TODO list",
	Todos: []Todo{
		{Title: "Task 1", Done: false},
		{Title: "Task 2", Done: true},
		{Title: "Task 3", Done: true},
	},
}
{{< / highlight >}}
{{< highlight html >}}
<h1>{{.PageTitle}}<h1>
<ul>
    {{range .Todos}}
        {{if .Done}}
            <li class="done">{{.Title}}</li>
        {{else}}
            <li>{{.Title}}</li>
        {{end}}
    {{end}}
</ul>
{{< / highlight >}}

## Control Structures
模板语言包含一组丰富的控制结构来渲染HTML.这里你将会看到一些最常用的。更详细的清单访问: <a target="_blank" href="https://golang.org/pkg/text/template/#hdr-Actions">text/template</a>

Control Structure | Definition
---|---
`{{/* a comment */}}` | Defines a comment
`{{.}}` | Renders the root element
`{{.Title}}` | Renders the "Title"-field in a nested element
`{{if .Done}} {{else}} {{end}}` | Defines an if-Statement
`{{range .Todos}} {{.}} {{end}}` | Loops over all "Todos" and renders each using `{{.}}`
`{{block "content" .}} {{end}}` | Defines a block with the name "content"

## Parsing Templates from Files
模板可以是从一个字符串或者磁盘上的文件解析。通常下，模板是来自磁盘。这个example 展示了如何来操作。在这个例子，下面GO程序中，有一个模板名字为`layout.html`.

{{< highlight go >}}
tmpl, err := template.ParseFiles("layout.html")
// or
tmpl := template.Must(template.ParseFiles("layout.html"))
{{< / highlight >}}

## Execute a Template in a Request Handler
模板从磁盘解析后，就可以在请求处理程序中使用它。`Execute ` 函数接受一个写入模板的`io.Writer`和一个将数据传递到模板中的 interface {} 。当被一个http.ResponseWriter 调用时. Header的Content-Type中的`Content-Type: text/html; charset=utf-8` 是被自动设置在HTTP 响应文中.

{{< highlight go >}}
func(w http.ResponseWriter, r *http.Request) {
	tmpl.Execute(w, "data goes here")
}
{{< / highlight >}}

## The Code (for copy/paste)
下面是全部代码.
{{< highlight go >}}
package main

import (
	"html/template"
	"net/http"
)

type Todo struct {
	Title string
	Done  bool
}

type TodoPageData struct {
	PageTitle string
	Todos     []Todo
}

func main() {
	tmpl := template.Must(template.ParseFiles("layout.html"))

	http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
		data := TodoPageData{
			PageTitle: "My TODO list",
			Todos: []Todo{
				{Title: "Task 1", Done: false},
				{Title: "Task 2", Done: true},
				{Title: "Task 3", Done: true},
			},
		}
		tmpl.Execute(w, data)
	})

	http.ListenAndServe(":80", nil)
}
{{< / highlight >}}
{{< highlight html >}}
<h1>{{.PageTitle}}<h1>
<ul>
    {{range .Todos}}
        {{if .Done}}
            <li class="done">{{.Title}}</li>
        {{else}}
            <li>{{.Title}}</li>
        {{end}}
    {{end}}
</ul>
{{< / highlight >}}
