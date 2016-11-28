---
layout: article
date: 2016-07-17
project-date: Nov 2016
category: articles
title: Markdown Cheatsheet
---

**Markdown** is a lightweight markup language with plain text formatting syntax designed so that it can be converted to HTML 
and many other formats using a tool by the same name. Markdown is often used to format readme files, for writing messages in 
online discussion forums, and to create rich text using a plain text editor.

Below are the most common used markdowns:

Table of Contents:<br>
<ul>
	<li class="page-scroll">
		<a href="#headers">Headers</a><br>
	</li>
	<li class="page-scroll">
		<a href="#emphasis">Emphasis</a><br>
	</li>
	<li class="page-scroll">
		<a href="#lists">Lists</a><br>
	</li>
	<li class="page-scroll">
		<a href="#links">Links</a><br>
	</li>
	<li class="page-scroll">
		<a href="#images">Images</a><br>
	</li>
	<li class="page-scroll">
		<a href="#code">Code</a><br>
	</li>
	<li class="page-scroll">
		<a href="#tables">Tables</a><br>
	</li>
	<li class="page-scroll">
		<a href="#blockquotes">Blockquotes</a><br>
	</li>
	<li class="page-scroll">
		<a href="#horizontal">Horizontal Rule</a><br>
	</li>
	<li class="page-scroll">
		<a href="#backslash">Backslash</a><br>
	</li>
</ul>



<section id="headers">
	<h1>Headers</h1>
</section>
```java
# H1
## H2
### H3
#### H4
##### H5
###### H6
```

# H1
## H2
### H3
#### H4
##### H5
###### H6



<section id="emphasis">
	<h1>Emphasis</h1>
</section>
```java
Emphasis, aka italics, with *asterisks* or _underscores_.

Strong emphasis, aka bold, with **asterisks** or __underscores__.
```

Emphasis, aka italics, with *asterisks* or _underscores_.

Strong emphasis, aka bold, with **asterisks** or __underscores__.



<section id="lists">
	<h1>Lists</h1>
</section>
Bullet List:

```java
* Item
* Item
```

* Item
* Item

Numbered List:

```java
1. Item
2. Item
```

1. Item
2. Item

Mix List:

```
1. Item
2. Item
    * Item
```

1. Item
2. Item
	* Item



<section id="links">
	<h1>Links</h1>
</section>
```java
[I'm an inline-style link](https://www.google.com)
```

[I'm an inline-style link](https://www.google.com)



<section id="images">
	<h1>Images</h1>
</section>
```java
Inline-style:

![alt text](https://github.com/adam-p/markdown-here/raw/master/src/common/images/icon48.png "Logo Title Text 1")

Reference-style: 

![alt text][logo]

[logo]: https://github.com/adam-p/markdown-here/raw/master/src/common/images/icon48.png "Logo Title Text 2"
```

Inline-style: 
![alt text](https://github.com/adam-p/markdown-here/raw/master/src/common/images/icon48.png "Logo Title Text 1")

Reference-style: 
![alt text][logo]

[logo]: https://github.com/adam-p/markdown-here/raw/master/src/common/images/icon48.png "Logo Title Text 2"



<section id="code">
	<h1>Code</h1>
</section>
```java
Inline `code` has `back-ticks around` it.
```

Inline `code` has `back-ticks around` it.

Blocks of code are fenced by lines with three back-ticks ```
```java
```javascript
var s = "JavaScript syntax highlighting";
alert(s);
```

```javascript
var s = "JavaScript syntax highlighting";
alert(s);
```



<section id="tables">
	<h1>Tables</h1>
</section>
There must be at least 3 dashes separating each header cell. The outer pipes (|) are optional, and you don't need to make the raw Markdown 
line up prettily. You can also use inline Markdown.

```java
Markdown | Less | Pretty
--- | --- | ---
*Still* | `renders` | **nicely**
1 | 2 | 3
```

Markdown | Less | Pretty
--- | --- | ---
*Still* | `renders` | **nicely**
1 | 2 | 3



<section id="blockquotes">
	<h1>Blockquotes</h1>
</section>
```java
> Blockquotes are very handy in email to emulate reply text.
> This line is part of the same quote.

Quote break.

> This is a very long line that will still be quoted properly when it wraps. Oh boy let's keep writing to make sure this is long enough to actually wrap for everyone. Oh, you can *put* **Markdown** into a blockquote. 
```

> Blockquotes are very handy in email to emulate reply text.
> This line is part of the same quote.

Quote break.

> This is a very long line that will still be quoted properly when it wraps. Oh boy let's keep writing to make sure this is long enough to actually wrap for everyone. Oh, you can *put* **Markdown** into a blockquote. 



<section id="horizontal">
	<h1>Horizontal Rule</h1>
</section>
```java
Three or more...

---

Hyphens

***

Asterisks

___

Underscores
```

Three or more...

---

Hyphens

***

Asterisks

___

Underscores


<section id="backslash">
	<h1>Backslash</h1>
</section>
Use "\\" to generate literal characters which would otherwise have special meaning in Markdown’s formatting syntax.



For more complete info, see [John Gruber's original spec](http://daringfireball.net/projects/markdown/)

