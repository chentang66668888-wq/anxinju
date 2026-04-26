<template>
  <div class="status-bar">
    <div class="status-left">
      <span><span class="status-dot green-dot"></span> 本地数据库：已连接</span>
      <span><span class="status-dot green-dot"></span> 毫米波雷达：在线</span>
      <span><span class="status-dot green-dot"></span> 心电图传感器：正常</span>
      <span><span class="status-dot blue-dot"></span> 路径图引擎：活跃</span>
    </div>
    <div class="sync-time">🕒 同步时间：2026-04-21 14:35:22 &nbsp;|&nbsp; 成功拉取高新区居家养老数据</div>
  </div>
</template>

<script>
export default { name: 'StatusBar' }
</script>

<style scoped>
/* rely on parent styles */
</style>
