readable-blob-stream
====================
Read W3C Blob & File objects as a Node stream. Very useful with
"[browserify](https://www.npmjs.org/package/browserify)" and "[primus](https://www.npmjs.org/package/primus)" with
"[ejson](https://www.npmjs.org/package/ejson)" using pipe().

INSTALLING
----------
If you are already generating a bundle using browserify (or something similar), you can use it directly:

    npm install readable-blob-stream --save

```javascript
var ReadableBlobStream = require('readable-blob-stream');
```

Or, you can generate a standalone javascript file by [cloning](https://github.com/Joris-van-der-Wel/readable-blob-stream.git) or
[downloading](https://github.com/Joris-van-der-Wel/readable-blob-stream/releases) this repo and typing:

    npm install
    npm run bundle

Look in the build directory to find the generated file. This is a
[UMD](http://dontkry.com/posts/code/browserify-and-the-universal-module-definition.html) module.
Which means you can either require() it using browserify, load it using AMD, or access it as the global `window.ReadableBlobStream`

EXAMPLE
-------
```html
<!DOCTYPE html>
<html>
<head>
<title>test</title>
<script src="readable-blob-stream.js"></script>
<script>
window.addEventListener('DOMContentLoaded', function()
{
    var myfile = document.getElementById('myfile');

    myfile.addEventListener('change', function()
    {
        var file = myfile.files[0];
        if (!file) { return; }

        var stream = new ReadableBlobStream(file);
        // or:
        //var stream = new ReadableBlobStream(file, {highWaterMark : 128, encoding: 'utf8'});

        stream.on('error', function(err)
        {
            console.log('error while reading your file:', err);
        });

        stream.on('end', function()
        {
            console.log('there will be no more data.');
        });

        stream.on('data', function(data)
        {
            // if you do not set an encoding,
            // "data" is both a Buffer and an Uint8Array
            console.log('got %d amount of data: ', data.length, data);
        });

        // If you are using primus you can simply use:
        //     stream.pipe(spark);
        // instead of using the 'data' listener
    });
});
</script>
</head>
<body>
<p>hi!</p>
<input type="file" id="myfile">
</body>
</html>
```