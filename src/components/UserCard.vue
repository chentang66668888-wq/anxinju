<template>
  <div class="user-card glass-card">
    <div class="user-avatar">👵</div>
    <div class="user-name">张德海</div>
    <div class="user-detail">
      <p>🎂 年龄：78岁</p>
      <p>🩺 身体状况：高血压、轻度认知障碍、夜间需关注如厕</p>
      <p>📌 注意事项：跌倒高风险时段 00:00-06:00</p>
    </div>
    <div class="setting-buttons">
      <div class="btn-outline">👤 更改使用者信息</div>
      <div class="btn-outline">❤️ 心电图设置</div>
      <div class="btn-outline">🗺️ 路径图设置</div>
    </div>
  </div>
</template>

<script>
export default { name: 'UserCard' }
</script>

<style scoped>
/* small tweaks can go here if needed */
</style>
