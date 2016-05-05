---
layout: post
title: "xxx"

published: false
author: kvz
---

<http://support.transloadit.com/discussions/questions/95472-add-cache-control-to-access-control-allow-headers>
Shervin Aflatooni <mailto:shervin.aflatooni@envato.com>

Hey Kevin,

Thanks, so I've actually open sourced the uploader I've been working on yesterday, it can be found here: <https://github.com/envato/react-studio-uploader/>

However its not really usable without our asset service which we wrote (which after a bit of cleaning up could also be open sourced, but may be a little while away).

However if you look though this file: <https://github.com/envato/react-studio-uploader/blob/master/src/index.jsx> you should be able to see how we are hooking dropzone up to transloadit.

We are basically doing the following:

this.dropzone = new Dropzone(this.refs.uploader.getDOMNode(), { 
      url: "<http://api2.transloadit.com/assemblies">; 
});

this.dropzone.on('sending', function(file, xhr, formData) { 
      formData.append('signature', TRANSLOADIT_SIGNATURE); 
      formData.append('params', TRANSLOADIT_PARAMS); 
});

Also we needed to make the modification to the Dropzone lib because of the CORS error I explained above, however if you fix that it will no longer be needed.

We sometimes (at Envato) write technical blog posts on our site: <http://webuild.envato.com/> so if I get around to it I could blog about our upload workflow in Envato studio and how it integrates with transloadit. If I do this I will be sure to hit you up about it, do you have a personal email I could contact you with?

Let me know if you want further help with dropzone!

Thanks, 
Shervin

<https://9a4210f09f81031c122f0f1cb303522a4941713c-www.googledrive.com/host/0B_SVOjiw8Ps5ejAyMEI2S3FFakk>
Ben Hirsch <mailto:benhirsch42@gmail.com>

Using Dropzone with Transloadit
1) The URL

Uploading with Dropzone is like using any regular form. We just have to format Dropzone's post the way Transloadit wants, and that starts with posting to the right place. While Dropzone fields can be created in HTML, I chose to do it all with CoffeeScript so our Dropzone object would be readily at hand.
@dropzone = new Dropzone '.dropzone', url: '<http://api2.transloadit.com/assemblies>'
2) The Params

To make a post, Transloadit needs an auth key and a Template ID. These can be appended to the request as it's being send with Dropzone's "sending" event.
@dropzone.on 'sending', (file, xhr, formData) ->
  params = JSON.stringify
    auth: {key: TRANSLOADIT_AUTH_KEY},
    template_id: TRANSLOADIT_TEMPLATE_ID
  formData.append 'params', params

That's it! We can upload to Transloadit with Dropzone. But how do we know when we're done?
3) Assembly Status

Normally, Transloadit's plugin listens for our file to finish processing for us, but since we're using Dropzone, we need to do that work ourselves. First, we need to know when Dropzone has finished uploading to Transloadit. We can do that with Dropzone's "success" event, which is fired every time it finishes uploading a file. This is an important distinction, because Dropzone supports uploading multiple files simultaneously.
@dropzone.on 'success', (file, response) =>
  @postURLfrom response.assembly_url
Then, we need a way to check the returned Assembly URL periodically and tell us when our file is ready:
postURLfrom: (assemblyURL) ->
  setTimeout =>
    $.get assemblyURL, (data) =>
      if data.ok == 'ASSEMBLY_COMPLETED'
        url = data.results[':original'][0].url
        @success url
      else
        @postURLfrom assemblyURL
  , 500

If the file is done processing, we extract the new URL to which our file was uploaded and pass it to a method called "success." Otherwise, we wait half a second and try again. This snippet does not accommodate Assembly failures.
4) Make It Pretty

From this point on, everything works exactly as expected. Dropzone's API supports everything from previews to progress bars. Here's how mine turned out.

Ben Hirsch
<http://www.sideqik.com/>
