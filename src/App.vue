<script setup>
import AgoraRTC from "agora-rtc-sdk-ng";

// appid：0b6ad43dafc944209fbf74426128086e
// apptoken：房间列表session中agora_token字段
// uid：99999999

let rtc = {
  localAudioTrack: null,
  client: null,
};

let options = {
  // Pass your App ID here.
  appId: "0b6ad43dafc944209fbf74426128086e",
  // Set the channel name.
  channel: "test",
  // Pass your temp token here.
  token: "0060b6ad43dafc944209fbf74426128086eIACRRNBgxVIXBCKD1fBfpvejaRfbxMWnUrvp+4DO3J6AZQFqBKmqlwe6EACPDOEBUutgYwEAAQAAAAAA",
  // Set the user ID.
  uid: 99999999,
};

async function startBasicCall() {
  // Create an AgoraRTCClient object.
  rtc.client = AgoraRTC.createClient({ mode: "rtc", codec: "vp8" });

  // Listen for the "user-published" event, from which you can get an AgoraRTCRemoteUser object.
  rtc.client.on("user-published", async (user, mediaType) => {
    // Subscribe to the remote user when the SDK triggers the "user-published" event
    await rtc.client.subscribe(user, mediaType);
    console.log("subscribe success");

    // If the remote user publishes an audio track.
    if (mediaType === "audio") {
      // Get the RemoteAudioTrack object in the AgoraRTCRemoteUser object.
      const remoteAudioTrack = user.audioTrack;
      // Play the remote audio track.
      remoteAudioTrack.play();
    }

    // Listen for the "user-unpublished" event
    rtc.client.on("user-unpublished", async (user) => {
      // Unsubscribe from the tracks of the remote user.
      await rtc.client.unsubscribe(user);
    });
  });

}
startBasicCall()

const join = async () => {
  // Join an RTC channel.
  await rtc.client.join(
    options.appId,
    options.channel,
    options.token,
    options.uid
  );
  // Create a local audio track from the audio sampled by a microphone.
  rtc.localAudioTrack = await AgoraRTC.createMicrophoneAudioTrack();
  // Publish the local audio tracks to the RTC channel.
  await rtc.client.publish([rtc.localAudioTrack]);

  console.log("publish success!");
};
const leave = async () => {
  // Destroy the local audio track.
  rtc.localAudioTrack.close();

  // Leave the channel.
  await rtc.client.leave();
};
</script>

<template>
  <div id="elementID"></div>
  <button @click="join">进入房间</button>
  <button @click="leave">离开房间</button>
</template>
