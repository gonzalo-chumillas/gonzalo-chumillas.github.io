---
layout: post
title:  "HTMLParser, a plugin to traverse and manage HTML documents"
date:   2017-01-20 21:07:00 +0200
categories: jekyll
---
[HTMLParser](https://github.com/soloproyectos-js/jquery.htmlparser) is a plugin to traverse, manage, fix and transform HTML documents. It was originally developed by [Erik Arvidsson](http://erik.eae.net/simplehtmlparser/simplehtmlparser.js) and I adapted it as a jQuery plugin and fixed some issues.

**Traversing and managing HTML documents was never so easy!**
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
