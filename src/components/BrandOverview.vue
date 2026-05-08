<template>
  <div class="brand-overview-panel">
    <div class="overview-header">
      <span class="header-icon">🏆</span>
      <h2>安芯居已经做到了以下几点</h2>
      <div class="header-decoration"></div>
    </div>
    
    <div class="overview-body">
      <div class="left-grid">
        <div class="stat-card" v-for="(item, idx) in stats" :key="idx">
          <div class="stat-glow"></div>
          <div class="stat-content">
            <div class="stat-title">{{ item.title }}</div>
            <div class="stat-value">{{ item.value }}</div>
          </div>
        </div>
      </div>

      <div class="right-column">
        <div class="big-card highlight-1">
          <div class="card-glow"></div>
          <div class="big-value">500</div>
          <div class="big-label">三年服务独居老人户数</div>
        </div>
        <div class="big-card highlight-2">
          <div class="card-glow"></div>
          <div class="big-value">70.42</div>
          <div class="big-label">营业利润率</div>
        </div>
        <div class="big-card highlight-3">
          <div class="card-glow"></div>
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
  gap: 24px;
  padding: 32px;
  background: linear-gradient(135deg, rgba(255, 255, 255, 0.08) 0%, rgba(255, 255, 255, 0.03) 100%);
  border-radius: 24px;
  border: 1px solid rgba(255, 255, 255, 0.12);
  backdrop-filter: blur(20px);
  box-shadow: 
    0 12px 32px rgba(0, 0, 0, 0.25),
    inset 0 1px 0 rgba(255, 255, 255, 0.1);
}

/* ===== 头部区域 ===== */
.overview-header {
  color: #dfe8ff;
  font-size: 1.4rem;
  font-weight: 700;
  text-align: center;
  position: relative;
  padding-bottom: 20px;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 12px;
}

.header-icon {
  font-size: 1.6rem;
  filter: drop-shadow(0 2px 6px rgba(0, 0, 0, 0.3));
}

.overview-header h2 {
  margin: 0;
  font-size: inherit;
  font-weight: inherit;
}

.header-decoration {
  position: absolute;
  bottom: 0;
  left: 50%;
  transform: translateX(-50%);
  width: 100px;
  height: 3px;
  background: linear-gradient(90deg, transparent 0%, #3b6eff 30%, #8b5cf6 70%, transparent 100%);
  border-radius: 2px;
}

/* ===== 主体内容 ===== */
.overview-body {
  display: flex;
  gap: 32px;
  align-items: stretch;
}

.left-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 20px;
  flex: 1;
}

.stat-card {
  position: relative;
  background: linear-gradient(180deg, rgba(255, 255, 255, 0.06), rgba(255, 255, 255, 0.02));
  padding: 24px 20px;
  border-radius: 16px;
  min-height: 140px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  text-align: center;
  border: 1px solid rgba(255, 255, 255, 0.08);
  transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
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
  opacity: 0.8;
}

.stat-glow {
  position: absolute;
  top: -50%;
  right: -50%;
  width: 200px;
  height: 200px;
  background: radial-gradient(circle, rgba(94, 224, 176, 0.15) 0%, transparent 70%);
  opacity: 0;
  transition: opacity 0.4s ease;
  pointer-events: none;
}

.stat-card:hover {
  transform: translateY(-6px);
  box-shadow: 
    0 12px 32px rgba(0, 0, 0, 0.25),
    0 0 24px rgba(94, 224, 176, 0.2);
  border-color: rgba(94, 224, 176, 0.3);
}

.stat-card:hover .stat-glow {
  opacity: 1;
}

.stat-content {
  position: relative;
  z-index: 1;
}

.stat-title {
  color: #bfe8ff;
  font-size: 0.95rem;
  margin-bottom: 12px;
  font-weight: 600;
  letter-spacing: 0.3px;
}

.stat-value {
  font-size: 2.2rem;
  font-weight: 800;
  color: #9fe8ff;
  text-shadow: 0 2px 8px rgba(159, 232, 255, 0.3);
  line-height: 1;
}

/* ===== 右侧大卡片 ===== */
.right-column {
  width: 300px;
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.big-card {
  position: relative;
  background: linear-gradient(135deg, rgba(255, 255, 255, 0.08), rgba(59, 110, 255, 0.05));
  padding: 28px 24px;
  border-radius: 20px;
  text-align: center;
  border: 1px solid rgba(255, 255, 255, 0.1);
  transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
  overflow: hidden;
  flex: 1;
}

.card-glow {
  position: absolute;
  top: -50%;
  right: -50%;
  width: 250px;
  height: 250px;
  background: radial-gradient(circle, var(--glow-color, rgba(255, 211, 106, 0.15)) 0%, transparent 70%);
  opacity: 0;
  transition: opacity 0.4s ease;
  pointer-events: none;
}

.big-card::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  height: 4px;
  background: var(--accent-gradient, linear-gradient(90deg, #ffd36a, #ffb142));
}

.big-card:hover {
  transform: translateY(-4px);
  box-shadow: 
    0 10px 28px rgba(0, 0, 0, 0.25),
    0 0 24px var(--glow-color, rgba(255, 211, 106, 0.2));
  border-color: var(--border-color, rgba(255, 211, 106, 0.3));
}

.big-card:hover .card-glow {
  opacity: 1;
}

.highlight-1 {
  --accent-gradient: linear-gradient(90deg, #ffd36a, #ffb142);
  --glow-color: rgba(255, 211, 106, 0.15);
  --border-color: rgba(255, 211, 106, 0.3);
}

.highlight-2 {
  --accent-gradient: linear-gradient(90deg, #3b6eff, #8b5cf6);
  --glow-color: rgba(59, 110, 255, 0.15);
  --border-color: rgba(59, 110, 255, 0.3);
}

.highlight-3 {
  --accent-gradient: linear-gradient(90deg, #5ee0b0, #26de81);
  --glow-color: rgba(94, 224, 176, 0.15);
  --border-color: rgba(94, 224, 176, 0.3);
}

.big-value {
  font-size: 3.2rem;
  font-weight: 800;
  color: var(--value-color, #ffd36a);
  margin-bottom: 10px;
  text-shadow: 0 2px 8px rgba(0, 0, 0, 0.2);
  position: relative;
  z-index: 1;
}

.highlight-1 .big-value { --value-color: #ffd36a; }
.highlight-2 .big-value { --value-color: #9fb4ff; }
.highlight-3 .big-value { --value-color: #5ee0b0; }

.big-label {
  color: #cfe0ff;
  font-size: 0.95rem;
  font-weight: 600;
  position: relative;
  z-index: 1;
  letter-spacing: 0.3px;
}

/* 响应式设计 */
@media (max-width: 1200px) {
  .brand-overview-panel {
    flex-direction: column;
  }

  .right-column {
    width: 100%;
    flex-direction: row;
    gap: 16px;
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
    padding: 24px;
  }

  .left-grid {
    grid-template-columns: 1fr;
  }

  .right-column {
    flex-direction: column;
    gap: 16px;
  }

  .stat-card {
    padding: 20px;
    min-height: 120px;
  }

  .stat-value {
    font-size: 1.9rem;
  }

  .big-value {
    font-size: 2.6rem;
  }
  
  .overview-header {
    font-size: 1.2rem;
  }
}
</style>