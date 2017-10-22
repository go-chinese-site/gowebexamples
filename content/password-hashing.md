+++
weight = 10
title = "Password Hashing"
description = "This example will show how to hash passwords using bcrypt in the Go programming language."
+++

# Password Hashing (bcrypt)

这个例子将展示如何使用bcrypt 来hash 密码.
为了使用它，我们需要 `go get`  bcrypt 这个库:

`$ go get golang.org/x/crypto/bcrypt`

接下来，我们所有的应用都将使用这个库..

{{< edison >}}

{{< highlight go >}}
// passwords.go
package main

import (
	"fmt"

	"golang.org/x/crypto/bcrypt"
)

func HashPassword(password string) (string, error) {
	bytes, err := bcrypt.GenerateFromPassword([]byte(password), 14)
	return string(bytes), err
}

func CheckPasswordHash(password, hash string) bool {
	err := bcrypt.CompareHashAndPassword([]byte(hash), []byte(password))
	return err == nil
}

func main() {
	password := "secret"
	hash, _ := HashPassword(password) // ignore error for the sake of simplicity

	fmt.Println("Password:", password)
	fmt.Println("Hash:    ", hash)

	match := CheckPasswordHash(password, hash)
	fmt.Println("Match:   ", match)
}

{{< / highlight >}}
{{< highlight console >}}
$ go run passwords.go
Password: secret
Hash:     $2a$14$ajq8Q7fbtFRQvXpdCq7Jcuy.Rx1h/L4J60Otx.gyNLbAYctGMJ9tK
Match:    true
{{< / highlight >}}
