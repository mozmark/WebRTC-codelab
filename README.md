#WebRTC tutorial

##Overview

WebRTC enables real-time communication in the browser.

This tutorial explains how to build a simple video and text chat application.

For more information about WebRTC, see [Getting started with WebRTC](http://www.html5rocks.com/en/tutorials/webrtc/basics) on HTML5 Rocks.

## Prerequisites

Basic knowledge:
1. git
2. Chrome Dev Tools

Installed on your development machine:
1. Chrome or Firefox Nightly
2. Code editor
3. Web server such as [MAMP](http://mamp.info/en/downloads) or [XAMPP](http://apachefriends.org/en/xampp.html)
4. Web cam
5. The source code: download or clone with git from  [source](https://bitbucket.org/webrtc/codelab/src).



## Step 1: Create a blank HTML5 document

Complete example: [code/step1.html]().

1. Create a bare-bones HTML document.

## Step 2: Get video from your webcam

Complete example: [code/step2.html]().

1. Add a video element to your page.
2. Add code to use getUserMedia() to set the source of the video from the web cam.
3. View your page from _localhost_.

`getUserMedia` is called like this:

    navigator.getUserMedia(constraints, successCallback, errorCallback);

The constraints argument allows us to specify the media to get, in this case video only:

    var constraints = {"video": true}

If successful, the video stream from the webcam is set as the source of the video element:

    function successCallback(localMediaStream) {
      window.stream = localMediaStream; // stream available to console
      var video = document.querySelector("video");
      video.src = window.URL.createObjectURL(localMediaStream);
      video.play();
    }

### Bonus points

1. Inspect the stream object from the console.
2. Try calling `stream.stop()`.
3. What does `stream.getVideoTracks()` return?
4. What size is the video element?  How can you get the video's natural size from JavaScript? Use the Chrome Dev Tools to check. Use CSS to make the video full width. How would you ensure the video is no higher than the viewport?
5. Try adding CSS filters to the video element (more ideas [here](http://html5-demos.appspot.com/static/css/filters/index.html)):

<pre>
video {
  filter: hue-rotate(180deg) saturate(200%);
  -moz-filter: hue-rotate(180deg) saturate(200%);
  -webkit-filter: hue-rotate(180deg) saturate(200%);
}
</pre>



## Step 3: Stream video with RTCPeerConnection

Complete example: [code/step3.html]().

This example sets up a connection between peers on the same page. Not much use, but good for understanding how RTCPeerConnection works!

1. Get rid of the JavaScript you've entered so far -- we're going to do something different!

2. Edit the HTML so there are two video elements and three buttons: Start, Call and Hang Up:

<pre>
&lt;video id="vid1" autoplay&gt;&lt;/video&gt;
&lt;video id="vid2" autoplay&gt;&lt;/video&gt;

&lt;div&gt;
  &lt;button id="startButton"&gt;Start&lt;/button&gt;
  &lt;button id="callButton"&gt;Call&lt;/button&gt;
  &lt;button id="hangupButton"&gt;Hang Up&lt;/button&gt;
&lt;/div&gt;
</pre>

3. Add the JavaScript from [code/step3.html]().

This code does a lot:

* Get the local description: a description (in SDP format) of local media conditions.
* Get a local ICE candidate: network information.
* Exchange descriptions and candidates.
* Pass the local stream to the remotePeerConnection.

### Bonus points

1. Take a look at _chrome://webrtc-internals_. (There is a full list of Chrome URLs at _chrome://about_.)
2. Style the page with CSS:
    - Put the videos side by side.
    - Make the buttons the same width, with bigger text.
3. From the Chrome Dev Tools console, inspect _localStream_, _localPeerConnection_ and _remotePeerConnection_. What does SDP format look like?



