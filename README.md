# transloadit-dropzonejs
Example integration of transloadit.com with dropzonejs.com

## Transloadit.com

Transloadit.com is a wonderful tool that takes the complexities and safety wories out of handling user-submitted images and files. It allows you to specify a set of rules in json format, and then let transloadit's "robots" do all the heavy lifting for you. For example, to upload and create a square thumbnail you would give the resize robot the following instruction:

```
"thumb": {
      "robot": "/image/resize", // the robot you want to use
      "use": ":orginal", // the file you want to use
      "width": 200,
      "height": 200,
      "resize_strategy": "crop" // crop the file
    }
```

Nothing revolutionary, but it gets a lot nicer once you start combining different robots into an "assembly". With just a few lines of json, you can upload an image, create a resized version and a thumbnail, strip their metadata, optimize them for the web, add a watermark and upload them to Amazon S3 or any remote FTP server! That's where it gets pretty awesome :)

## Dropzonejs

While the Transloadit.com website features a plethora of examples on how to integrate Transloadit into your projects using javascript/jQuery, php or ruby, I was wondering if it would be possible to integrate Transloadit's backend with the Dropzonejs upload interface. Because javascript is not my native language, I'm sharing the code here, so you can use and improve it :) If you have any improvements or suggestions, send me a pull request!

## How to use it?

All you need to do is fill in your public key and template_id where it reads:

```
// configure below
```

## How does it work?

To adapt Dropzonejs to work with Transloadit, all you need to do is append Transloadit's config array to the form on submit:

```
 sending: function(file, xhr, formData) {
      formData.append('params', params); // params contains our transloadit config
 }
```

Yeah, that was also easier than I had thought... ;)

## Getting the assembly status

If we would leave it at this, we would only receive a status update once the upload has finished. We would never hear from our beloved robots again, and have no way of knowing the status of the transcoding-process (assembly). Just dead silence.

In order to know how our robots are getting along, we need to actively check the status of our assembly. To do so, we simply keep sending requests to the assembly url (which was sent to us after the upload finished) every so often, until we receive the status "ASSEMBLY_COMPLETED". The returned json array contains all the info we need (filename, url, etc.). This is done in the checkStatus(assembly_url); function.

With this data, we can then end the upload and present the images our robots lovingly crafted for us.
