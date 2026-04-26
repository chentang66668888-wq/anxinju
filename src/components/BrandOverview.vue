<template>
  <div class="brand-overview-panel">
    <div class="overview-header">安芯居已经做到了以下几点</div>
    <div class="overview-body">
      <div class="left-grid">
      <div class="stat-card" v-for="(item, idx) in stats" :key="idx">
        <div class="stat-title">{{ item.title }}</div>
        <div class="stat-value">{{ item.value }}</div>
        <div class="stat-sub" v-if="item.sub">{{ item.sub }}</div>
      </div>
      </div>

      <div class="right-column">
      <div class="big-card">
        <div class="big-value">500</div>
        <div class="big-label">三年服务独居老人户数</div>
      </div>
      <div class="big-card">
        <div class="big-value">70.42</div>
        <div class="big-label">营业利润率</div>
      </div>
      <div class="big-card">
        <div class="big-value">295.53</div>
        <div class="big-label">累计营业利润</div>
      </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'BrandOverview',
  data() {
    return {
      stats: [
        { title: '降低跌倒致死率', value: '60% 以上' },
        { title: '挽救严重安全事故', value: '15-20 起' },
        { title: '节约家庭照护时长', value: '30-45 万小时' },
        { title: '用户满意度达', value: '95% 以上' },
        { title: '预警响应时间', value: '≤ 5 分钟' },
        { title: '设备正常运行率', value: '≥ 98%' },
        { title: '算法误报率', value: '≤ 0.5%' },
        { title: '志愿者参与', value: '200+ 人次' },
        { title: '合作社区', value: '50+ 个' }
      ]
    }
  }
}
</script>

<style scoped>
.brand-overview-panel { display:flex; flex-direction:column; gap:12px }
.overview-header { color:#dfe8ff; font-size:1.05rem; font-weight:700; padding:8px 4px }
.overview-body { display:flex; gap:24px; align-items:flex-start }
.left-grid { display:grid; grid-template-columns: repeat(3, 1fr); gap:18px; flex:1 }
.stat-card { background: linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01)); padding:18px; border-radius:12px; min-height:120px; display:flex; flex-direction:column; justify-content:center }
.stat-title { color:#bfe8ff; font-size:1rem; margin-bottom:8px }
.stat-value { font-size:2.2rem; font-weight:800; color:#9fe8ff }
.stat-sub { color:#9aa9c7; margin-top:6px }
.right-column { width:260px; display:flex; flex-direction:column; gap:24px }
.big-card { background: linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01)); padding:24px; border-radius:18px; text-align:center }
.big-value { font-size:3.2rem; font-weight:800; color:#ffd36a }
.big-label { color:#cfe0ff; margin-top:8px }

@media (max-width:1200px) {
  .brand-overview-panel { flex-direction:column }
  .right-column { width:100%; flex-direction:row; gap:12px }
  .left-grid { grid-template-columns: repeat(2, 1fr) }
}
</style>
