#WebRTC tutorial

##Overview

WebRTC enables real-time communication in the browser.

This tutorial explains how to build a simple video and text chat application.

For more information about WebRTC, see [Getting started with WebRTC](http://www.html5rocks.com/en/tutorials/webrtc/basics) on HTML5 Rocks.

## Prerequisites

Basic knowledge:

1. [git](http://git-scm.com/)
2. [Chrome Dev Tools](https://developers.google.com/chrome-developer-tools/)

Installed on your development machine:

1. Chrome or Firefox Nightly
2. Code editor
3. Web server such as [MAMP](http://mamp.info/en/downloads) or [XAMPP](http://apachefriends.org/en/xampp.html)
4. Web cam
5. git, in order to get the source code
6. The [source code](https://bitbucket.org/webrtc/codelab/src)

It would also be useful to have an Android device with Chrome Beta installed in order to try out the examples on mobile.

## Step 1: Create a blank HTML5 document

Complete example: [complete/step1.html](https://bitbucket.org/webrtc/codelab/src/9681a4376644/complete/step1.html).

1. Create a bare-bones HTML document.

## Step 2: Get video from your webcam

Complete example: [complete/step2.html](https://bitbucket.org/webrtc/codelab/src/9681a4376644/complete/step2.html).

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

Complete example: [complete/step3.html](https://bitbucket.org/webrtc/codelab/src/9681a4376644/complete/step3.html).

RTCPeerConnection is the WebRTC API for video and audio calling.

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

3. Add the JavaScript from [complete/step3.html](https://bitbucket.org/webrtc/codelab/src/9681a4376644/complete/step3.html).

This code does a lot!:

* Get and share local and remote descriptions: metadata (in SDP format) of local media conditions.
* Get and share ICE candidates: network information.
* Pass the local stream to the remote _RTCPeerConnection_.

### Bonus points

1. Take a look at _chrome://webrtc-internals_. (There is a full list of Chrome URLs at _chrome://about_.)
2. Style the page with CSS:
    - Put the videos side by side.
    - Make the buttons the same width, with bigger text.
    - Make sure it works on mobile.
3. From the Chrome Dev Tools console, inspect _localStream_, _localPeerConnection_ and _remotePeerConnection_.
4. Take a look at _localPeerConnection.localDescription_. What does SDP format look like?

## Step 4: Stream arbitrary data with RTCDataChannel

Complete example: [complete/step4.html](https://bitbucket.org/webrtc/codelab/src/9681a4376644/complete/step4.html).

For this step, we'll create an app that sends text between two textareas on the same page. Not very useful, except to demonstrate how RTCDataChannel works!

1. Create a new document and add the following HTML:

<pre>
&lt;textarea id="dataChannelSend" disabled&gt;&lt;/textarea&gt;
&lt;textarea id="dataChannelReceive" disabled&gt;&lt;/textarea&gt;

&lt;div id="buttons"&gt;
  &lt;button id="startButton" onclick="createConnection()"&gt;Start&lt;/button&gt;
  &lt;button id="sendButton" onclick="sendData()"&gt;Send&lt;/button&gt;
  &lt;button id="closeButton" onclick="closeDataChannels()"&gt;Stop&lt;/button&gt;
&lt;/div&gt;
</pre>

3. Add the JavaScript from [complete/step4.html](https://bitbucket.org/webrtc/codelab/src/9681a4376644/complete/step3.html).

This code uses RTCPeerConnection to enable exchange of text messages. The additional code is as follows:

    function sendData(){
      var data = document.getElementById("dataChannelSend").value;
      sendChannel.send(data);
    }
    localPeerConnection = new webkitRTCPeerConnection(servers,
      {optional: [{RtpDataChannels: true}]});
    sendChannel = localPeerConnection.createDataChannel("sendDataChannel",
      {reliable: false});
    ...
    remotePeerConnection = new webkitRTCPeerConnection(servers,
      {optional: [{RtpDataChannels: true}]});
    function receiveChannelCallback(event) {
      receiveChannel = event.channel;
      receiveChannel.onmessage = onReceiveMessageCallback;
    }
    remotePeerConnection.ondatachannel = receiveChannelCallback;
    function onReceiveMessageCallback(event) {
      document.getElementById("dataChannelReceive").value = event.data;
    }

    Notice the use of constraints.

### Bonus points

1. Try out RTCDataChannel file sharing with [Sharefest](http://www.sharefest.me/). When would RTCDataChannel need to be reliable, and when might performance be more important?
2. Use CSS to improve page layout.
3. Add a placeholder attribute to the _dataChannelReceive_ textarea.
4. Test the page on a mobile device.


## Step 4: Set up a signalling server

## Step 5: Exchange messages via the signalling server

## Step 7: putting it all together: RTCDataChannel + RTCPeerConnection





