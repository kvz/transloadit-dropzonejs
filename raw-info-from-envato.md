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

