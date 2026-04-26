<template>
  <div class="route-card glass-card">
    <div class="route-title">路线图</div>
    <div class="route-plot">路线图显示区域（示意）</div>
  </div>
</template>

<script>
export default { name: 'AnxRouteMap' }
</script>

<style scoped>
.route-card { padding:18px; display:flex; flex-direction:column; gap:12px; min-height:220px; }
.route-title { font-size:1.1rem; color:#dfe8ff; font-weight:600; text-align:center }
.route-plot { flex:1; background: linear-gradient(180deg, rgba(6,12,20,0.06), rgba(8,14,26,0.02)); border-radius:12px; display:flex; align-items:center; justify-content:center; color:#9fb4ff; font-size:1.2rem; }
</style>
