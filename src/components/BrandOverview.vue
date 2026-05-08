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
  gap: 20px;
  padding: 25px;
  background: rgba(255, 255, 255, 0.08);
  border-radius: 20px;
  border: 1px solid rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(15px);
  box-shadow: 0 8px 25px rgba(0, 0, 0, 0.2);
}

.overview-header {
  color: #dfe8ff;
  font-size: 1.2rem;
  font-weight: 700;
  padding: 15px 0;
  text-align: center;
  position: relative;
}

.overview-header::after {
  content: '';
  position: absolute;
  bottom: 0;
  left: 50%;
  transform: translateX(-50%);
  width: 80px;
  height: 3px;
  background: linear-gradient(90deg, #3b6eff, #8b5cf6);
  border-radius: 2px;
}

.overview-body {
  display: flex;
  gap: 30px;
  align-items: flex-start;
}

.left-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 20px;
  flex: 1;
}

.stat-card {
  background: linear-gradient(180deg, rgba(255, 255, 255, 0.05), rgba(255, 255, 255, 0.02));
  padding: 20px;
  border-radius: 15px;
  min-height: 130px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  text-align: center;
  border: 1px solid rgba(255, 255, 255, 0.08);
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
  height: 3px;
  background: linear-gradient(90deg, #5ee0b0, #26de81);
}

.stat-card:hover {
  transform: translateY(-5px);
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.2);
  border-color: rgba(94, 224, 176, 0.3);
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
  color: #9fe8ff;
  margin-bottom: 5px;
}

.stat-sub {
  color: #9aa9c7;
  margin-top: 8px;
  font-size: 0.85rem;
  font-weight: 500;
}

.right-column {
  width: 280px;
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.big-card {
  background: linear-gradient(135deg, rgba(255, 255, 255, 0.08), rgba(59, 110, 255, 0.05));
  padding: 25px;
  border-radius: 18px;
  text-align: center;
  border: 1px solid rgba(255, 255, 255, 0.1);
  transition: all 0.3s ease;
  position: relative;
  overflow: hidden;
}

.big-card::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  height: 4px;
  background: linear-gradient(90deg, #ffd36a, #ffb142);
}

.big-card:hover {
  transform: translateY(-3px);
  box-shadow: 0 8px 25px rgba(255, 211, 106, 0.2);
  border-color: rgba(255, 211, 106, 0.3);
}

.big-value {
  font-size: 3.5rem;
  font-weight: 800;
  color: #ffd36a;
  margin-bottom: 8px;
  text-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.big-label {
  color: #cfe0ff;
  font-size: 1rem;
  font-weight: 600;
}

@media (max-width: 1200px) {
  .brand-overview-panel {
    flex-direction: column;
  }

  .right-column {
    width: 100%;
    flex-direction: row;
    gap: 15px;
  }

  .left-grid {
    grid-template-columns: repeat(2, 1fr);
  }

  .big-card {
    flex: 1;
  }
}

@media (max-width: 768px) {
  .brand-overview-panel {
    padding: 20px;
  }

  .left-grid {
    grid-template-columns: 1fr;
  }

  .right-column {
    flex-direction: column;
    gap: 15px;
  }

  .stat-card {
    padding: 18px;
    min-height: 110px;
  }

  .stat-value {
    font-size: 2rem;
  }

  .big-value {
    font-size: 2.8rem;
  }
}
</style>
