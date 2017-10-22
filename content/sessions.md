+++
weight = 7
title = "Sessions"
description = "This example will show how to store data in session cookies using the popular gorilla/sessions package in the Go programming language."
+++

# Sessions

这个例子将会展示如何通过流行的 <a target="_blank" href="https://github.com/gorilla/sessions">gorilla/sessions</a> 会话 cookies.

cookie 是存储在用户浏览器中的小数据，并在每次请求时发送到服务器。在它们中，我们可以存储用户是否登录到我们的网站，并找出它时间上是谁（在我们系统中.

在本例中，我们只允许经过身份验证在`/secret` 页上查看隐私的信息。要访问它，首先要访问`/login`得到有效的会话cookie.此外，他可以访问`/logout` 来注销我们访问私密信息的权限.

{{< edison >}}

{{< highlight go >}}
// sessions.go
package main

import (
	"fmt"
	"net/http"

	"github.com/gorilla/sessions"
)

var (
	// key must be 16, 24 or 32 bytes long (AES-128, AES-192 or AES-256)
	key = []byte("super-secret-key")
	store = sessions.NewCookieStore(key)
)

func secret(w http.ResponseWriter, r *http.Request) {
	session, _ := store.Get(r, "cookie-name")

	// Check if user is authenticated
	if auth, ok := session.Values["authenticated"].(bool); !ok || !auth {
		http.Error(w, "Forbidden", http.StatusForbidden)
		return
	}

	// Print secret message
	fmt.Fprintln(w, "The cake is a lie!")
}

func login(w http.ResponseWriter, r *http.Request) {
	session, _ := store.Get(r, "cookie-name")

	// Authentication goes here
	// ...

	// Set user as authenticated
	session.Values["authenticated"] = true
	session.Save(r, w)
}

func logout(w http.ResponseWriter, r *http.Request) {
	session, _ := store.Get(r, "cookie-name")

	// Revoke users authentication
	session.Values["authenticated"] = false
	session.Save(r, w)
}

func main() {
	http.HandleFunc("/secret", secret)
	http.HandleFunc("/login", login)
	http.HandleFunc("/logout", logout)

	http.ListenAndServe(":8080", nil)
}
{{< / highlight >}}
{{< highlight console >}}
$ go run sessions.go

$ curl -s http://localhost:8080/secret
Forbidden

$ curl -s -I http://localhost:8080/login
Set-Cookie: cookie-name=MTQ4NzE5Mz...

$ curl -s --cookie "cookie-name=MTQ4NzE5Mz..." http://localhost:8080/secret
The cake is a lie!
{{< / highlight >}}


