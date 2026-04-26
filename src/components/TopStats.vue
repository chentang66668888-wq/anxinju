<template>
  <div class="stat-card glass-card">
    <div class="stat-title">🏠 摔倒次数</div>
    <div class="stat-value">3 <span style="font-size: 1.2rem;">次</span></div>
    <div class="stat-trend"><span class="trend-up">▲ +33%</span> <span class="badge">较上周</span></div>
  </div>
  <div class="stat-card glass-card">
    <div class="stat-title">🛡️ 居家安全评估</div>
    <div class="stat-value">86<span style="font-size: 1.2rem;"> 分</span></div>
    <div class="stat-trend">良好 · 综合风险低</div>
  </div>
  <div class="stat-card glass-card">
    <div class="stat-title">🌫️ 空气质量</div>
    <div class="stat-value">42 <span style="font-size: 1rem;">µg/m³</span></div>
    <div class="stat-trend">PM2.5 轻度 · 建议通风</div>
  </div>
  <div class="stat-card glass-card">
    <div class="stat-title">🚽 如厕时间 / 频次</div>
    <div class="stat-value">6 <span style="font-size: 1rem;">次/日均</span></div>
    <div class="stat-trend">夜间如厕 2 次 · 时长正常</div>
  </div>
</template>

<script>
export default {
  name: 'TopStats'
}
</script>

<style scoped>
/* keeps layout inside stats-row */
.stat-card { min-height:120px; }
</style>
