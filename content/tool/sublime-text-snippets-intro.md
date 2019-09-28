---
title: "Sublime Text使用代码片段Snippets高效写作"
date: 2019-09-24T21:47:24+08:00
type: posts
categories: [工具]
keywords: ["sublime text"]
tags: ["sublime text"]
---

Snippets是可以复用的代码片段模板，可以帮助你快速输入大量重复内容，极大提高写作效率。
<!--more-->
在Sublime Text中新建Snippet，选择菜单：Tools->Developer->New Snippet...，Sublime Text会生成一个Snippet模板。

<img src="/tool/sublime-text-snippets-intro/1.png" style="width:600px;"/>

```
<snippet>
	<content><![CDATA[
Hello, ${1:this} is a ${2:snippet}.
]]></content>
	<!-- Optional: Set a tabTrigger to define how to trigger the snippet -->
	<!-- <tabTrigger>hello</tabTrigger> -->
	<!-- Optional: Set a scope to limit where the snippet will trigger -->
	<!-- <scope>source.python</scope> -->
</snippet>

```

将模板内容修改如下：
```
<snippet>
	<content><![CDATA[
Hello, this is a snippet.
]]></content>
	<!-- Optional: Set a tabTrigger to define how to trigger the snippet -->
	<tabTrigger>hello</tabTrigger>
	<!-- Optional: Set a scope to limit where the snippet will trigger -->
	<scope>source.python</scope>
</snippet>

```

简单介绍一下各节点含义（看不懂没关系，后面的实际操作会让你豁然开朗，可以回头再看`^_^`）：

* `content`节点内容表示实际的代码片段。
* `tabTrigger`节点内容表示用来输入代码片段的快捷字符串。
* `scope`节点内容表示代码片段会在哪种上下文环境下激活, 比如上面代码定义了source.python, 意思是这段代码片段会在python语言环境下激活。**注意：Scope不是文件扩展名，可以从菜单：Tools->Developer->Show Scope Name查看当前文件扩展名对应的Scope。**

把修改后的模板保存为文件`hello.sublime-snippet`（保存目录保持默认打开的目录即可）。

<img src="/tool/sublime-text-snippets-intro/2.png" style="width:600px;"/>

新建文件`t.py`，在文件中输入`hello`，按下`Tab`键，则输出如下：
<img src="/tool/sublime-text-snippets-intro/3.png" style="width:600px;"/>

好了，这就是Snippet最基本的用法，是不是很简单很强大！更多高级功能请参考官方文档[http://docs.sublimetext.info/en/latest/extensibility/snippets.html](http://docs.sublimetext.info/en/latest/extensibility/snippets.html)

**一个常见问题：针对Markdown文件的Snippet没有出现在自动补全列表中**

解决办法是选择菜单Preferences->Settings，在右侧的窗口中增加如下设置：
```
"auto_complete_selector": "meta.tag - punctuation.definition.tag.begin, source - comment - string.quoted.double.block - string.quoted.single.block - string.unquoted.heredoc, text",
```

附常见语言对应Scope值：
```
ActionScript: source.actionscript.2
AppleScript: source.applescript
ASP: source.asp
Batch FIle: source.dosbatch
C#: source.cs
C++: source.c++
Clojure: source.clojure
CoffeeScript: source.coffee
CSS: source.css
D: source.d
Diff: source.diff
Erlang: source.erlang
Go: source.go
GraphViz: source.dot
Groovy: source.groovy
Haskell: source.haskell
HTML: text.html(.basic)
JSP: text.html.jsp
Java: source.java
Java Properties: source.java-props
Java Doc: text.html.javadoc
JSON: source.json
Javascript: source.js
BibTex: source.bibtex
Latex Log: text.log.latex
Latex Memoir: text.tex.latex.memoir
Latex: text.tex.latex
LESS: source.css.less
TeX: text.tex
Lisp: source.lisp
Lua: source.lua
MakeFile: source.makefile
Markdown: text.html.markdown
Multi Markdown: text.html.markdown.multimarkdown
Matlab: source.matlab
Objective-C: source.objc
Objective-C++: source.objc++
OCaml campl4: source.camlp4.ocaml
OCaml: source.ocaml
OCamllex: source.ocamllex
Perl: source.perl
PHP: source.php
Regular Expression(python): source.regexp.python
Python: source.python
R Console: source.r-console
R: source.r
Ruby on Rails: source.ruby.rails
Ruby HAML: text.haml
SQL(Ruby): source.sql.ruby
Regular Expression: source.regexp
RestructuredText: text.restructuredtext
Ruby: source.ruby
SASS: source.sass
Scala: source.scala
Shell Script: source.shell
SQL: source.sql
Stylus: source.stylus
TCL: source.tcl
HTML(TCL): text.html.tcl
Plain text: text.plain
Textile: text.html.textile
XML: text.xml
XSL: text.xml.xsl
YAML: source.yaml
```
