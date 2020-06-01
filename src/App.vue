<template>
  <div id="app">
    <button @click="openApp">唤醒pc客户端</button>
  </div>
</template>

<script>
export default {
  name: "App",
  methods: {
    openApp() {
      this.callapp_PC({
        // 这个url地址换成需要唤醒的客户端的URL Protoco
        // 客户端自定义URL Protoco的设置请看我的博客 pc前端js调起电脑本地应用程序(需要客户端配合 自定义URL Protocol 协议 )
        url: "baijiacloud://urlpath=https%3A%2F%2Fb41568785.at.baijiayun.com",
        callback: function() {
          console.log("唤醒失败了,做一些唤醒失败后的操作吧");
        },
      });
    },
    callapp_PC({ url, callback }) {
      var t = setTimeout(callback, 1000);
      var inp = document.createElement("input");
      inp.style.position = "absolute";
      inp.style.clip = "rect(0, 0, 0, 0)";
      function blur() {
        window.clearTimeout(t);
      }
      inp.addEventListener("blur", blur); // 监听blur事件
      document.body.appendChild(inp);
      inp.focus(); // 获取焦点
      setTimeout(function() {
        // 删除无用的标签
        inp.removeEventListener("blur", blur);
        document.body.removeChild(inp);
      }, 1000);
      //有客户端 如果有本地exe应用，就会弹框，input失去焦点，然后执行blur()事件 清空t定时器 - 删除无用的标签定时器
      //无客户端 不会弹框，input也不会失去焦点 触发t定时器- 执行callback - 删除无用的标签定时器
      // https://blog.csdn.net/weixin_44309374?orderby=UpdateTime
      location.href = url;
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
