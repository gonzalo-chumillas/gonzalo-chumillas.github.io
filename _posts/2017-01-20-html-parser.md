---
layout: post
title:  "HTMLParser, un plugin para leer y transformar documentos HTML"
date:   2017-01-20 21:07:00 +0200
categories: jekyll
---
[HTMLParser](https://github.com/soloproyectos-js/jquery.htmlparser) es un plugin para leer, manipular, arreglar y transformar documentos HTML. Origininalmente fue desarrollado por [Erik Arvidsson](http://erik.eae.net/simplehtmlparser/simplehtmlparser.js), pero necesitaba un plugin en jQuery y quería solucionar algunos aspectos del código fuente.

**Leer y manipular documentos nunca fue tan fácil :)**
```js
// Example 2: parses an HTML document
var html =
    '<p>Actually <strong>we do not exist</strong>.<br />' +
    'But before we can prove it, <em>we will have already disappeared.</em></p>';
$.htmlParser(html, {
        start: function () {
            // 'this' is a jQuery object representing the current node
            console.log('Start tag: <' + this.prop('tagName') + '>');
        },
        end: function () {
            console.log('End tag: </' + this.prop('tagName') + '>');
        },
        text: function () {
            console.log('Text: ' + this.text());
        },
        comment: function (text) {
            console.log('Comment: ' + this.text());
        }
});
```

[HTMLParser](https://github.com/soloproyectos-js/jquery.htmlparser)
