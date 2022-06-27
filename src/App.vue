<template>
  <div id="app">
    <router-view />
    <audio class="audio" controls src="@/assets/order.mp3" ref="audio"></audio>
  </div>
</template>

<script>
export default {
  name: "App",
  created() {
    this.WebSocketinit();
  },
  methods: {
    // 创建socket
    WebSocketinit() {
      let that = this;
      if ("WebSocket" in window) {
        // 打开一个 web socket
        this.ws = new WebSocket("ws://47.111.178.57:8282");
        this.ws.onopen = () => {
          console.log("链接成功");
        };

        this.ws.onmessage = function(evt) {
          var received_msg = evt.data;
          received_msg = JSON.parse(received_msg);
          if (received_msg.type == "msg") {
            console.log(received_msg);
            that.$notify({
              title: "订单提示",
              message: `${received_msg.msg}`,
              // message: "10号桌，订阅某某某书，请处理",
              type: "success",
              duration: 0
            });

            that.$refs["audio"].paly();
          }
        };
        this.ws.onclose = function() {
          // 关闭 websocket
        };
      } else {
        // 浏览器不支持 WebSocket
        alert("您的浏览器不支持 WebSocket!");
      }
    }
  }
};
</script>

<style lang="scss" scoped>
.audio {
  position: absolute;
  // top: 9999px;
  top: 10px;
  z-index: 99996;
}
</style>
