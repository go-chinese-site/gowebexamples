# Go Web Examples

[Go Web Examples](https://gowebexamples.com/) (中文翻译版，在线阅读地址：http://books.studygolang.com/gowebexamples/)
提供了简单易懂的代码片段，介绍如何在 go 中进行 web 开发, 这是受 [Go By Example](http://gobyexample.com/) (中文翻译版，在线阅读地址：http://books.studygolang.com/gobyexample/)启发，它对于了解该语言的基础知识有很大帮助。

## Examples

当前有以下主题的教程和示例：

- [Hello World](http://books.studygolang.com/gowebexamples/hello-world/)
- [Routing (using gorilla/mux)](http://books.studygolang.com/gowebexamples/routes-using-gorilla-mux/)
- [Templates](http://books.studygolang.com/gowebexamples/templates/)
- [Requests and Forms](http://books.studygolang.com/gowebexamples/forms/)
- [Assets and Files](http://books.studygolang.com/gowebexamples/static-files/)
- [Basic Middleware](http://books.studygolang.com/gowebexamples/basic-middleware/)
- [Advanced Middleware](http://books.studygolang.com/gowebexamples/advanced-middleware/)
- [Sessions](http://books.studygolang.com/gowebexamples/sessions/)
- [JSON](http://books.studygolang.com/gowebexamples/json/)
- [Websockets](http://books.studygolang.com/gowebexamples/websockets/)
- [Password Hashing (bcrypt)](http://books.studygolang.com/gowebexamples/password-hashing/)

## 构建

该站点是使用 [Hugo](https://github/spf13/hugo) 静态站点生成器构建的。
这些示例的源代码可以在 `content` 目录中找到。
使用 `hugo` 命令生成站点时, 输出文件将生成到 `public` 目录中。

要构建这个网站，必须安装 Hugo 和 Python (安装了 `pygments`) 。

```console
$ git clone https://github.com/go-chinese-site/gowebexamples
$ cd gowebexamples
$ hugo
```

## 翻译

中文翻译工作由 [Go 语言中文网](https://studygolang.com) 组织，[ryan](https://github.com/ryan-bin)、[polaris](https://github.com/polaris1119) 翻译。

## 感谢

感谢 [Go By Example](https://gobyexample.com/)，有它才有了这个项目。

## License

MIT
