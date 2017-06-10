# Image Resizer for Cordova #
By: Protonet GmbH

# This fork

This fork uses the **cordova-plugin-file** folder as the default. 

You'll get a path ending in **"/files/files/yourfilename.jpg"** (note the the "files/files")

You can read the files written by this plugin by requesting an entry like this:

```
function getFileEntryP(opts) {

    return new Promise((resolve, reject)=> {
        window.requestFileSystem(window.LocalFileSystem.PERSISTENT, opts.size || 1024 * 1024, function (fs) {
            console.log('file system open: ' + fs.name);
            fs.root.getFile(opts.fileName, {create: true, exclusive: false}, function (fileEntry) {
                resolve(fileEntry)
            }, reject);

        }, reject);
    })
}

function _readFileP(fileEntry, method = 'readAsText') {

    return new Promise((resolve, reject)=> {
        fileEntry.file(function (file) {
            var reader = new FileReader();

            reader.onloadend = function () {
                method === 'readAsText' && console.log("Successful file read: ", this.result);
                resolve(this.result)
            };

            reader[method](file);

        }, reject);
    })
}

```


Authors: Joschka Schulz

## Adding the Plugin ##

Use the Cordova CLI and type in the following command:

`cordova plugin add https://github.com/protonet/cordova-plugin-image-resizer.git`

## Sample Code

At the moment the plugin is available on android, iOS and windows

### resize

    window.ImageResizer.resize(options, success, failed);

### Options
  - **uri**(String): The Uri for the image on the device to get scaled
  - **folderName**(String): The name of the folder the image should be put in **android only**
  - **fileName**(String): A custom name for the file. Default name is a timestamp. **android and windows only**
  - **quality**(Number): Quality given as Number for the quality of the new image **android and iOS only**
  - **width**(Number): The width of the new image,
  - **height**(Number): The height of the new image

### Android Example
    var options = {
          uri: uri,
          folderName: "Protonet Messenger",
          quality: 90,
          width: 1280,
          height: 1280};

    window.ImageResizer.resize(options,
      function(image) {
         // success: image is the new resized image
      }, function() {
        // failed: grumpy cat likes this function
      });
