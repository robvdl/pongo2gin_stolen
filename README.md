Pongo2gin
=========

Package pongo2gin is a template renderer that can be used with the Gin web
framework https://github.com/gin-gonic/gin it uses the Pongo2 template library
https://github.com/flosch/pongo2

The original code can be found on https://gitlab.com/go-box/pongo2gin this repository
was copied a few years ago and all the original authors and copyright was shamelessly stripped.

This is now a copy of that stolen code since I asked for the first copy to be taken down,
please send patches to the original repo or rename this repo to something else,
and please credit the original author. Don't make this out to be your own work.

pongo2 is a Django-syntax like templating-language (official website).

## Installation  

`go get "github.com/tecome/pongo2gin"`

Requirements
------------

Requires Gin 1.14 or higher and Pongo2.

Usage
-----

To use pongo2gin you need to set your router.HTMLRenderer to a new renderer
instance, this is done after creating the Gin router when the Gin application
starts up. This assumes templates will be located in the "templates"
directory, or you can use pongo2gin.TemplatePath("templates") to specify a custom location.

To render templates from a route, call c.HTML just as you would with
regular Gin templates, the only difference is that you pass template
data as a pongo2.Context instead of gin.H type.


![Screen](https://raw.githubusercontent.com/tecome/pongo2gin/master/example/ginScreen.png)

Basic Example
-------------

```go
package main
import (
	"log"
	"net/http"
	"github.com/tecome/pongo2gin"
	"github.com/flosch/pongo2"
	"github.com/gin-gonic/gin"
)
//GetAllData all list
func GetAllData(c *gin.Context) {
	posts := []string{
		"Larry Ellison",
		"Carlos Slim Helu",
		"Mark Zuckerberg",
		"Amancio Ortega ",
		"Jeff Bezos",
		" Warren Buffet ",
		"Bill Gates",
		"selman tunç",
	}
	// Call the HTML method of the Context to render a template
	c.HTML(http.StatusOK, "index.html",
		pongo2.Context{
			"title": "hello pongo",
			"posts": posts,
		},
	)
}

func main() {

	gin.SetMode(gin.DebugMode)
	r := gin.Default()
	r.Use(gin.Recovery())
	r.HTMLRender = pongo2gin.TemplatePath("templates")
	r.GET("home", GetAllData)
	log.Fatal(r.Run(":8888"))
}
```

HTML 
----------------


```html

<h1> {{ title }}</h1>

{% for post in posts%}
<ul>
  <li>{{post}}</li>
</ul>
{% endfor %}

```

Template Caching
----------------

Templates will be cached if the current Gin Mode is set to anything but "debug",
this means the first time a template is used it will still load from disk, but
after that the cached template will be used from memory instead.

If he Gin Mode is set to "debug" then templates will be loaded from disk on
each request.

Caching is implemented by the Pongo2 library itself.

GoDoc
-----


Specials Thanks
-----

https://zhuanlan.zhihu.com/p/105956154  

https://github.com/yuchanns
