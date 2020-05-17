---
layout: post
title:  "Use latex and github table style in my jekyll blog"
date:   2019-04-19 08:43:12 +0530
categories: latex jekyll
---
# For latex setting
## _config.yml
add the following commands into your _config.yml
```yml
markdown: kramdown
kramdown:
   math_engine: mathjax
```

## head.html in _includes

```html
<!-- Mathjax Support -->
<script type="text/javascript" async
  src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-MML-AM_CHTML">
</script>
```


Then it should be ready to show the latex contents in your blog.

# For table style

## _syntax.scss
Add the following css style into the file. I have copied it from the table style of github.

[get it here](https://gist.github.com/andyferra/2554919)

```css
table {
	padding: 0; }
	table tr {
	  border-top: 1px solid #cccccc;
	  background-color: white;
	  margin: 0;
	  padding: 0; }
	  table tr:nth-child(2n) {
		background-color: #f8f8f8; }
	  table tr th {
		font-weight: bold;
		border: 1px solid #cccccc;
		text-align: left;
		margin: 0;
		padding: 6px 13px; }
	  table tr td {
		border: 1px solid #cccccc;
		text-align: left;
		margin: 0;
		padding: 6px 13px; }
	  table tr th :first-child, table tr td :first-child {
		margin-top: 0; }
	  table tr th :last-child, table tr td :last-child {
		margin-bottom: 0; }
```