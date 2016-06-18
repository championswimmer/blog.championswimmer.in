---
layout: post
published: true
mathjax: false
featured: false
comments: false
title: Sending a streaming zip file in node.js
categories:
  - webdevelopment
tags: 'gsoc, web development, nodejs'
---
The webapp generator that [we set up](https://opev.wordpress.com/2016/06/06/webapp-the-generator-for-making-schedule-pages/) is now up and running on a [heroku instance](http://open-event-generator.herokuapp.com/) and is working to generate zips of generated websites.

One of the requirements was that when the front-end presses, the “Generate” button, a request is created to the backend node app to start creating the html pages, and pack them into a zip file. Then the said zip file will be made available to download.

I intended for the zip file to become available to download immediately and the server to continuously pack data into the zip, as the user downloads it – a process known as **streaming zip serving**. (If you might have noticed, Google Drive uses this too. You do not know the final zip size before the download has completed.)

If the server takes time in creating the contents of the zip, this method can help, as it will allow the user to start downloading the file, before all the contents are finalised.


To achieve the mentioned goal, the idea Node.js module to use **_archiver_**.

If you look at the [app.js](https://github.com/fossasia/open-event-webapp/blob/master/src/generator/app.js) code you’ll find on the /generate POST endpoint, we take the ‘req’ params and pass to a pipeToRes() function, which is defined in [generator.js](https://github.com/fossasia/open-event-webapp/blob/master/src/generator/backend/generator.js)

```javascript
app.post('/generate', uploadedFiles, function(req, res, next) {
  console.log(req.files);
  console.log(req.body);
  res.setHeader('Content-Type', 'application/zip');
  generator.pipeZipToRes(req, res);
});
```
And in the generator, you can find this block of code that pipes the streaming archive to the response.

```javascript
      const zipfile = archiver('zip');

      zipfile.on('error', (err) => {
        throw err;
      });

      zipfile.pipe(res);

      zipfile.directory(distHelper.distPath, '/').finalize();
      
```

We zip the directory defined in `distPath` and the archiver API has a fluent call to `.finalize()` that finished the download for the user, when the zip is completed.