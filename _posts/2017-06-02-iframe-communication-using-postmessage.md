---
layout: post
title: iFrame cross-domain communication using postMessage
---
### Introduction
Window.postMessage() is a great way to communicate cross domain between an iFrame and it's container. As always in the case of iFrames, the container and the frame should be executed on the same port. And that port should probably be 443 using https.

Still not using HTTPS?
Go to [Let's Encrypt](https://letsencrypt.org/) and get encrypted!

### Basic examples
Posting a message to container.
Note that the second parameter, url, must include protocol and must match the intended target for the message to be allowed.
```javascript
window.postMessage('Message from iFrame', 'https://container.url.com/', [transfer, optional sequence of transferable objects* to transfer to destination.]);
```

Listening for messages.
* Always check origin for security reasons
* Message is available in event.data
* event.source contains a reference to the calling party and can be used to establish two-way communication.

```javascript
window.addEventListener("message", myMessageHandler, false);

function myMessageHandler(event) {
	/* Always check the origin first to prevent injections */
    if (event.origin !== 'https://expected.url.com')
    	return;
    /* Supplied message will be present in event.data */
    alert('Got message: ' + event.data);
}
```

### Posting to the iFrame
Posting a message to the iFrame is done in the same way.
We only need to change the window object.
```html
<iframe id="messageTarget" src="https://target.url.com"></iframe>
<script type="text/javascript">
	document
    	.getElementById('messageTarget')
        .postMessage('Message to iFrame', 'https://target.url.com/');
</script>
```

\* Read more on Transferable objects at [mozilla.org](https://developer.mozilla.org/en-US/docs/Web/API/Transferable)