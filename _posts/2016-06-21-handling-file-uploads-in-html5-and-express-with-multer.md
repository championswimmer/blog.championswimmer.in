---
layout: post
published: true
mathjax: false
featured: false
comments: false
title: Handling File uploads in HTML5 and Express (with Multer)
tags: 'express, node, multer, web development, html5, forms'
categories:
  - webdevelopment
  - Node.js
  - GSOC
---
The [Open Event Webapp Generator](http://open-event-generator.herokuapp.com/) has a pure HTML front end form, and Express based backend.

 

We needed an option where users can upload their own JSON files to create a schedule page for it.

On the HTML form that’s easy. As you can see here we simple add input tags with type=”file”

```html
<input type="file" name="sponsorfile" id="sponsorfile">
```

In our express app, we need to use [multer](https://www.npmjs.com/package/multer) to be able to handle file uploads.

We create a middleware called uploadedFiles, and pass the middleware to the get() block

```javascript
var express = require('express');
var multer = require('multer');
var app = express();

var upload = multer({dest: 'uploads/'});

var uploadedFiles = upload.fields([
  {name: 'speakerfile', maxCount: 1},
  {name: 'sessionfile', maxCount: 1},
  {name: 'trackfile', maxCount: 1},
  {name: 'sponsorfile', maxCount: 1},
  {name: 'eventfile', maxCount: 1},
  {name: 'locationfile', maxCount: 1}
]);

app.post('/live', uploadedFiles, function(req, res) {
         // req.files has the files
         // req.body has the POST body params
  });
});
```

Now we can access the files inside `req.files` (that will have a path to the temporary location of the file, you’ll have to use some filesystem module to read the files), and the POST params in `req.body`
