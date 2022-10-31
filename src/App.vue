<script setup>
import AgoraRTC from "agora-rtc-sdk";

// appid：0b6ad43dafc944209fbf74426128086e
// apptoken：房间列表session中agora_token字段
// uid：99999999
const test = () => {
  console.log("test");
  let client = AgoraRTC.createClient({
    mode: "rtc",
    codec: "vp8",
  });
  client.init("0b6ad43dafc944209fbf74426128086e");
  // let remoteStream = AgoraRTC.createStream(streamSpec)
  client.join(
    "0060b6ad43dafc944209fbf74426128086eIACRRNBgxVIXBCKD1fBfpvejaRfbxMWnUrvp+4DO3J6AZQFqBKmqlwe6EACPDOEBUutgYwEAAQAAAAAA",
    "",
    "99999999"
  );
  // remoteStream.play('elementID')
  // client.leave()
};
</script>

<template>
  <div id="elementID"></div>
  <button @click="test">进入房间</button>
</template>
