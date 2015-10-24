---
layout: post
title: "Jekyll:Syntax Highlighting in Github Favoured Markdown codeblocks"
description: "How to enable Syntax Highlighting with GFM on Jekyll"
headline: "How to enable Syntax Highlighting with GFM on Jekyll"
modified: 2015-10-25 03:27:34 +0530
category: [Web, Programming]
tags: [Jekyll, 'Github Pages', Markdown, 'Syntax Highlighting']
imagefeature: 
mathjax: 
chart: 
comments: true
featured: false
---
You are using Jekyll to host a blog on Github pages. And you need to add Syntax Highlighting to your code. And you want to mark your code blocks using the triple backtick format of Github flavored markdown.

Now that's a very unique set of circumstances to be in, and if you are in those shoes, you might have figured out that it seems almost impossible to get this to work. Here are the basic blockers - 
1.  Github doesn't allow user plugins. So you cannot just put coderay.rb inside _plugins
2.  Github by default uses *redcarpet* as the markdown interpreter. But, *kramdown* has inbuilt syntax highlighting support, redcarpet doesn't
3.  Since `{% highlight ruby %} {% endhighlight %}` kind of code block demarcaters work, no one in the Jekyll/Redcarpet/Kramdown community wants to help fix this issue. 
4.  Redcarpet can interpret GFM, while Kramdown cannot. Although the documentation says it can.

But that aside, lets quickly take a look at how this can be solved. 
So Github uses *redcarpet* internally to parse markdown, and we should stick with it, because that's the only bet to successfully interpret GFM code blocks. 
Also, Github Pages has pygments installed already, so for syntax highlighting we **have to** use pygments only. 
So put the following lines in your _config.yml

```yaml
# Conversion
markdown: redcarpet
highlighter: pygments
redcarpet:
  extensions: ["no_intra_emphasis", "fenced_code_blocks", "autolink", "strikethrough", "superscript"]
```

Now here is a very important step. Having a pgments css file and having it included is **very** important. Even if all the other configs are ok, but the css not included, the Jekyll site wont build on Github's server. 

You can save the following as assets/css/pygments/solarized-dark.css

```css
/* Solarized Dark 

For use with Jekyll and Pygments

http://ethanschoonover.com/solarized

SOLARIZED HEX      ROLE
--------- -------- ------------------------------------------
base03    #002b36  background
base01    #586e75  comments / secondary content
base1     #93a1a1  body text / default code / primary content
orange    #cb4b16  constants
red       #dc322f  regex, special keywords
blue      #268bd2  reserved keywords
cyan      #2aa198  strings, numbers
green     #859900  operators, other keywords
*/

.highlight { background-color: #002b36; color: #93a1a1; max-width: 56.25rem; padding: 1rem; border-radius: 1rem; margin: 0px auto 2em; }
.highlight .lineno { color: #586e75 } /* Line Numbers */
.highlight .c { color: #586e75 } /* Comment */
.highlight .err { color: #93a1a1 } /* Error */
.highlight .g { color: #93a1a1 } /* Generic */
.highlight .k { color: #859900 } /* Keyword */
.highlight .l { color: #93a1a1 } /* Literal */
.highlight .n { color: #93a1a1 } /* Name */
.highlight .o { color: #859900 } /* Operator */
.highlight .x { color: #cb4b16 } /* Other */
.highlight .p { color: #93a1a1 } /* Punctuation */
.highlight .cm { color: #586e75 } /* Comment.Multiline */
.highlight .cp { color: #859900 } /* Comment.Preproc */
.highlight .c1 { color: #586e75 } /* Comment.Single */
.highlight .cs { color: #859900 } /* Comment.Special */
.highlight .gd { color: #2aa198 } /* Generic.Deleted */
.highlight .ge { color: #93a1a1; font-style: italic } /* Generic.Emph */
.highlight .gr { color: #dc322f } /* Generic.Error */
.highlight .gh { color: #cb4b16 } /* Generic.Heading */
.highlight .gi { color: #859900 } /* Generic.Inserted */
.highlight .go { color: #93a1a1 } /* Generic.Output */
.highlight .gp { color: #93a1a1 } /* Generic.Prompt */
.highlight .gs { color: #93a1a1; font-weight: bold } /* Generic.Strong */
.highlight .gu { color: #cb4b16 } /* Generic.Subheading */
.highlight .gt { color: #93a1a1 } /* Generic.Traceback */
.highlight .kc { color: #cb4b16 } /* Keyword.Constant */
.highlight .kd { color: #268bd2 } /* Keyword.Declaration */
.highlight .kn { color: #859900 } /* Keyword.Namespace */
.highlight .kp { color: #859900 } /* Keyword.Pseudo */
.highlight .kr { color: #268bd2 } /* Keyword.Reserved */
.highlight .kt { color: #dc322f } /* Keyword.Type */
.highlight .ld { color: #93a1a1 } /* Literal.Date */
.highlight .m { color: #2aa198 } /* Literal.Number */
.highlight .s { color: #2aa198 } /* Literal.String */
.highlight .na { color: #93a1a1 } /* Name.Attribute */
.highlight .nb { color: #B58900 } /* Name.Builtin */
.highlight .nc { color: #268bd2 } /* Name.Class */
.highlight .no { color: #cb4b16 } /* Name.Constant */
.highlight .nd { color: #268bd2 } /* Name.Decorator */
.highlight .ni { color: #cb4b16 } /* Name.Entity */
.highlight .ne { color: #cb4b16 } /* Name.Exception */
.highlight .nf { color: #268bd2 } /* Name.Function */
.highlight .nl { color: #93a1a1 } /* Name.Label */
.highlight .nn { color: #93a1a1 } /* Name.Namespace */
.highlight .nx { color: #93a1a1 } /* Name.Other */
.highlight .py { color: #93a1a1 } /* Name.Property */
.highlight .nt { color: #268bd2 } /* Name.Tag */
.highlight .nv { color: #268bd2 } /* Name.Variable */
.highlight .ow { color: #859900 } /* Operator.Word */
.highlight .w { color: #93a1a1 } /* Text.Whitespace */
.highlight .mf { color: #2aa198 } /* Literal.Number.Float */
.highlight .mh { color: #2aa198 } /* Literal.Number.Hex */
.highlight .mi { color: #2aa198 } /* Literal.Number.Integer */
.highlight .mo { color: #2aa198 } /* Literal.Number.Oct */
.highlight .sb { color: #586e75 } /* Literal.String.Backtick */
.highlight .sc { color: #2aa198 } /* Literal.String.Char */
.highlight .sd { color: #93a1a1 } /* Literal.String.Doc */
.highlight .s2 { color: #2aa198 } /* Literal.String.Double */
.highlight .se { color: #cb4b16 } /* Literal.String.Escape */
.highlight .sh { color: #93a1a1 } /* Literal.String.Heredoc */
.highlight .si { color: #2aa198 } /* Literal.String.Interpol */
.highlight .sx { color: #2aa198 } /* Literal.String.Other */
.highlight .sr { color: #dc322f } /* Literal.String.Regex */
.highlight .s1 { color: #2aa198 } /* Literal.String.Single */
.highlight .ss { color: #2aa198 } /* Literal.String.Symbol */
.highlight .bp { color: #268bd2 } /* Name.Builtin.Pseudo */
.highlight .vc { color: #268bd2 } /* Name.Variable.Class */
.highlight .vg { color: #268bd2 } /* Name.Variable.Global */
.highlight .vi { color: #268bd2 } /* Name.Variable.Instance */
.highlight .il { color: #2aa198 } /* Literal.Number.Integer.Long */
```

Or if you prefer light, please use this one instead

```css
/* Solarized Light 

For use with Jekyll and Pygments

http://ethanschoonover.com/solarized

SOLARIZED HEX      ROLE
--------- -------- ------------------------------------------
base01    #586e75  body text / default code / primary content
base1     #93a1a1  comments / secondary content
base3     #fdf6e3  background
orange    #cb4b16  constants
red       #dc322f  regex, special keywords
blue      #268bd2  reserved keywords
cyan      #2aa198  strings, numbers
green     #859900  operators, other keywords
*/

.highlight { background-color: #fdf6e3; color: #586e75; max-width: 56.25rem; padding: 1rem; border-radius: 1rem; margin: 0px auto 2em; }
.highlight .lineno { color: #93a1a1 } /* Line Numbers */
.highlight .c { color: #93a1a1 } /* Comment */
.highlight .err { color: #586e75 } /* Error */
.highlight .g { color: #586e75 } /* Generic */
.highlight .k { color: #859900 } /* Keyword */
.highlight .l { color: #586e75 } /* Literal */
.highlight .n { color: #586e75 } /* Name */
.highlight .o { color: #859900 } /* Operator */
.highlight .x { color: #cb4b16 } /* Other */
.highlight .p { color: #586e75 } /* Punctuation */
.highlight .cm { color: #93a1a1 } /* Comment.Multiline */
.highlight .cp { color: #859900 } /* Comment.Preproc */
.highlight .c1 { color: #93a1a1 } /* Comment.Single */
.highlight .cs { color: #859900 } /* Comment.Special */
.highlight .gd { color: #2aa198 } /* Generic.Deleted */
.highlight .ge { color: #586e75; font-style: italic } /* Generic.Emph */
.highlight .gr { color: #dc322f } /* Generic.Error */
.highlight .gh { color: #cb4b16 } /* Generic.Heading */
.highlight .gi { color: #859900 } /* Generic.Inserted */
.highlight .go { color: #586e75 } /* Generic.Output */
.highlight .gp { color: #586e75 } /* Generic.Prompt */
.highlight .gs { color: #586e75; font-weight: bold } /* Generic.Strong */
.highlight .gu { color: #cb4b16 } /* Generic.Subheading */
.highlight .gt { color: #586e75 } /* Generic.Traceback */
.highlight .kc { color: #cb4b16 } /* Keyword.Constant */
.highlight .kd { color: #268bd2 } /* Keyword.Declaration */
.highlight .kn { color: #859900 } /* Keyword.Namespace */
.highlight .kp { color: #859900 } /* Keyword.Pseudo */
.highlight .kr { color: #268bd2 } /* Keyword.Reserved */
.highlight .kt { color: #dc322f } /* Keyword.Type */
.highlight .ld { color: #586e75 } /* Literal.Date */
.highlight .m { color: #2aa198 } /* Literal.Number */
.highlight .s { color: #2aa198 } /* Literal.String */
.highlight .na { color: #586e75 } /* Name.Attribute */
.highlight .nb { color: #B58900 } /* Name.Builtin */
.highlight .nc { color: #268bd2 } /* Name.Class */
.highlight .no { color: #cb4b16 } /* Name.Constant */
.highlight .nd { color: #268bd2 } /* Name.Decorator */
.highlight .ni { color: #cb4b16 } /* Name.Entity */
.highlight .ne { color: #cb4b16 } /* Name.Exception */
.highlight .nf { color: #268bd2 } /* Name.Function */
.highlight .nl { color: #586e75 } /* Name.Label */
.highlight .nn { color: #586e75 } /* Name.Namespace */
.highlight .nx { color: #586e75 } /* Name.Other */
.highlight .py { color: #586e75 } /* Name.Property */
.highlight .nt { color: #268bd2 } /* Name.Tag */
.highlight .nv { color: #268bd2 } /* Name.Variable */
.highlight .ow { color: #859900 } /* Operator.Word */
.highlight .w { color: #586e75 } /* Text.Whitespace */
.highlight .mf { color: #2aa198 } /* Literal.Number.Float */
.highlight .mh { color: #2aa198 } /* Literal.Number.Hex */
.highlight .mi { color: #2aa198 } /* Literal.Number.Integer */
.highlight .mo { color: #2aa198 } /* Literal.Number.Oct */
.highlight .sb { color: #93a1a1 } /* Literal.String.Backtick */
.highlight .sc { color: #2aa198 } /* Literal.String.Char */
.highlight .sd { color: #586e75 } /* Literal.String.Doc */
.highlight .s2 { color: #2aa198 } /* Literal.String.Double */
.highlight .se { color: #cb4b16 } /* Literal.String.Escape */
.highlight .sh { color: #586e75 } /* Literal.String.Heredoc */
.highlight .si { color: #2aa198 } /* Literal.String.Interpol */
.highlight .sx { color: #2aa198 } /* Literal.String.Other */
.highlight .sr { color: #dc322f } /* Literal.String.Regex */
.highlight .s1 { color: #2aa198 } /* Literal.String.Single */
.highlight .ss { color: #2aa198 } /* Literal.String.Symbol */
.highlight .bp { color: #268bd2 } /* Name.Builtin.Pseudo */
.highlight .vc { color: #268bd2 } /* Name.Variable.Class */
.highlight .vg { color: #268bd2 } /* Name.Variable.Global */
.highlight .vi { color: #268bd2 } /* Name.Variable.Instance */
.highlight .il { color: #2aa198 } /* Literal.Number.Integer.Long */
```

Make sure to add `<link rel="stylesheet" href="{{ site.url }}/assets/css/pygments/solarized-light.css">` to the layout of your posts. Most probably might be in _includes/head.html or specifically in _layouts/post.html

That should be it. Your Jekyll blog posts, with GFM code blocks should now be highlighted in Solarized, if all went well. 
As a proof, I am using the same method in this very block. Feel free to check out the sources. 