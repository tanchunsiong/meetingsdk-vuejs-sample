<script setup>
import { ZoomMtg } from '@zoomus/websdk';

ZoomMtg.setZoomJSLib('https://source.zoom.us/2.18.2/lib', '/av');
ZoomMtg.preLoadWasm();
ZoomMtg.prepareWebSDK();
// loads language files, also passes any error messages to the ui
ZoomMtg.i18n.load('en-US');
ZoomMtg.i18n.reload('en-US');

var authEndpoint = ''
var sdkKey = ''
var meetingNumber = '123456789'
var passWord = ''
var role = 0
var userName = 'Vue.js'
var userEmail = ''
var registrantToken = ''
var zakToken = ''
var leaveUrl = 'http://localhost:5173/'

function getSignature() {
  fetch(authEndpoint, {
    method: 'POST',
    body: JSON.stringify({
      meetingNumber: meetingNumber,
      role: role
    })
  }).then((response) => {
    return response.json()
  }).then((data) => {
    console.log(data)
    startMeeting(data.signature)
  }).catch((error) => {
    console.log(error)
  })
}

function startMeeting(signature) {
  document.getElementById('zmmtg-root').style.display = 'block';

  ZoomMtg.init({
    leaveUrl: leaveUrl,
    success: (success) => {
      console.log(success);
      ZoomMtg.join({
        signature: signature,
        sdkKey: sdkKey,
        meetingNumber: meetingNumber,
        passWord: passWord,
        userName: userName,
        userEmail: userEmail,
        tk: registrantToken,
        zak: zakToken,
        success: (success) => {
          console.log(success);
        },
        error: (error) => {
          console.log(error);
        }
      });
    },
    error: (error) => {
      console.log(error);
    }
  });
}
</script>

<template>
  <h1>Zoom Meeting SDK Vue.js Sample</h1>

  <button @click='getSignature'>Join Meeting</button>
</template>

<style scoped>

</style>
