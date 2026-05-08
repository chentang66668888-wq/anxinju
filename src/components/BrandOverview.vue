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
.brand-overview-panel {
  display: flex;
  flex-direction: column;
  gap: 16px;
  padding: 24px;
  background: linear-gradient(135deg, rgba(255, 218, 106, 0.1), rgba(255, 193, 7, 0.1));
  border-radius: 24px;
  border: 1px solid rgba(255, 218, 106, 0.2);
  position: relative;
  overflow: hidden;
}
.brand-overview-panel::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: linear-gradient(135deg, rgba(255, 218, 106, 0.05), transparent);
  opacity: 0;
  transition: opacity 0.3s ease;
}
.brand-overview-panel:hover::before {
  opacity: 1;
}
.overview-header {
  color: #dfe8ff;
  font-size: 1.1rem;
  font-weight: 700;
  padding: 12px 8px;
  border-left: 4px solid #ffd36a;
  background: linear-gradient(90deg, rgba(255, 211, 106, 0.1), transparent);
  border-radius: 0 8px 8px 0;
}
.overview-body {
  display: flex;
  gap: 28px;
  align-items: flex-start;
}
.left-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 20px;
  flex: 1;
}
.stat-card {
  background: linear-gradient(135deg, rgba(255, 255, 255, 0.04), rgba(255, 255, 255, 0.01));
  padding: 20px;
  border-radius: 16px;
  min-height: 130px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  border: 1px solid rgba(255, 218, 106, 0.2);
  transition: all 0.3s ease;
  position: relative;
  overflow: hidden;
}
.stat-card::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: linear-gradient(135deg, rgba(255, 218, 106, 0.05), transparent);
  opacity: 0;
  transition: opacity 0.3s ease;
}
.stat-card:hover::before {
  opacity: 1;
}
.stat-card:hover {
  transform: translateY(-3px);
  box-shadow: 0 8px 20px rgba(255, 218, 106, 0.2);
}
.stat-title {
  color: #bfe8ff;
  font-size: 1rem;
  margin-bottom: 10px;
  font-weight: 600;
}
.stat-value {
  font-size: 2.4rem;
  font-weight: 800;
  color: #ffd36a;
  margin-bottom: 6px;
}
.stat-sub {
  color: #9aa9c7;
  margin-top: 8px;
  font-size: 0.85rem;
}
.right-column {
  width: 280px;
  display: flex;
  flex-direction: column;
  gap: 20px;
}
.big-card {
  background: linear-gradient(135deg, rgba(255, 218, 106, 0.15), rgba(255, 193, 7, 0.15));
  padding: 28px;
  border-radius: 20px;
  text-align: center;
  border: 1px solid rgba(255, 218, 106, 0.3);
  box-shadow: 0 6px 16px rgba(255, 218, 106, 0.2);
  transition: all 0.3s ease;
}
.big-card:hover {
  transform: translateY(-2px);
  box-shadow: 0 8px 20px rgba(255, 218, 106, 0.3);
}
.big-value {
  font-size: 3.5rem;
  font-weight: 800;
  color: #ffd36a;
  margin-bottom: 8px;
  text-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
}
.big-label {
  color: #cfe0ff;
  margin-top: 10px;
  font-size: 0.95rem;
  font-weight: 600;
}

@media (max-width:1200px) {
  .brand-overview-panel { flex-direction:column }
  .right-column { width:100%; flex-direction:row; gap:12px }
  .left-grid { grid-template-columns: repeat(2, 1fr) }
}
</style>
