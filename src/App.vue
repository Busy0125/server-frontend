<template>
  <div :style="bg">
    <div class="time-container">
      <div>{{ currentDate }}</div>
      <div>{{ currentTime }}</div>
    </div>
    <div class="upload-container">
      <el-upload
          drag
          action="/api/file/upload"
          accept=".xls,.xlsx"
          :on-success="handleFileUpload"
      >
        <div>
          将文件拖至此处或点击此处上传
        </div>
        <template #tip>
          <div style="color: white;">
            只允许上传xls和xlsx文件
          </div>
        </template>
      </el-upload>
    </div>
    <a href="https://beian.miit.gov.cn/" target="_blank">备案号：滇ICP备2024029954号-2</a>
  </div>
</template>

<script>
export default {
  data() {
    return {
      bg: {
        backgroundImage: "url(" + require("./assets/desktop.jpg") + ")",
        backgroundRepeat: 'no-repeat',
        backgroundSize: 'cover',  // 使用 cover 以确保背景图片覆盖整个容器
        position: 'absolute',     // 使用 absolute 以使其覆盖整个视窗
        width: '100%',
        height: '100%',
        backgroundPosition: 'center center'
      },
      currentDate: this.getFormattedTime(),
      currentTime: this.getFormattedTime()
    }
  },
  mounted() {
    this.update();
    this.timer = setInterval(this.update, 1000);
  },
  beforeDestroy() {
    clearInterval(this.timer);
  },
  methods: {
    update() {
      this.currentDate = this.getFormattedDate();
      this.currentTime = this.getFormattedTime();
    },
    getFormattedDate() {
      const now = new Date();
      const year = now.getFullYear();
      const month = String(now.getMonth() + 1).padStart(2, '0');
      const day = String(now.getDate()).padStart(2, '0');
      return `${year}-${month}-${day}`;
    },
    getFormattedTime() {
      const now = new Date();
      const hours = String(now.getHours()).padStart(2, '0');
      const minutes = String(now.getMinutes()).padStart(2, '0');
      const seconds = String(now.getSeconds()).padStart(2, '0');
      return `${hours}:${minutes}:${seconds}`;
    },
    handleFileUpload(response) {
      console.log('文件上传成功', response.data);
      // 创建一个链接元素
      const link = document.createElement('a');
      link.href = response.data; // 设置链接的地址为后端返回的下载链接
      link.target = '_blank'; // 在新标签页中打开链接
      link.download = 'output.docx'; // 设置下载文件的名称

      // 将链接添加到文档中
      document.body.appendChild(link);

      // 触发链接的点击事件，开始下载文件
      link.click();

      // 下载完成后移除链接元素
      document.body.removeChild(link);
    }
  }
}
</script>

<style>
.time-container {
  color: white;
  font-size: 100px;
  font-weight: bold;
  user-select: none;
}

.upload-container {
  width: 25%;
  margin: 50px 0 0 100px;
}

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box; /* 也可以加上这一行以便更好地控制盒模型 */
}
</style>