# transloadit-dropzonejs
Example integration of transloadit.com with dropzonejs.com


## A little background...

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

Nothing revolutionary, but it gets a lot nicer once you start combining different robots into an "assembly". With just a few lines of json, you can upload an image, create a resized version and a thumbnail, strip their metadata, optimize them for the web, and upload them to Amazon S3 or a remote FTP server! That's where it gets pretty awesome :)
