<template>
  <div class="stats-container">
    <div class="stat-card glass-card" v-for="(stat, index) in statsData" :key="index">
      <div class="stat-icon">{{ stat.icon }}</div>
      <div class="stat-content">
        <div class="stat-title">{{ stat.title }}</div>
        <div class="stat-value">
          {{ stat.value }}
          <span class="stat-unit" v-if="stat.unit">{{ stat.unit }}</span>
        </div>
        <div class="stat-trend" :class="stat.trendClass">
          <span class="trend-text">{{ stat.trend }}</span>
          <span class="trend-badge" v-if="stat.badge">{{ stat.badge }}</span>
        </div>
      </div>
      <div class="stat-glow"></div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'TopStats',
  data() {
    return {
      statsData: [
        {
          icon: '🏠',
          title: '摔倒次数',
          value: '3',
          unit: '次',
          trend: '▲ +33%',
          trendClass: 'trend-warning',
          badge: '较上周'
        },
        {
          icon: '🛡️',
          title: '居家安全评估',
          value: '86',
          unit: '分',
          trend: '良好 · 综合风险低',
          trendClass: 'trend-success',
          badge: ''
        },
        {
          icon: '🌫️',
          title: '空气质量',
          value: '42',
          unit: 'µg/m³',
          trend: 'PM2.5 轻度 · 建议通风',
          trendClass: 'trend-info',
          badge: ''
        },
        {
          icon: '🚽',
          title: '如厕时间 / 频次',
          value: '6',
          unit: '次/日均',
          trend: '夜间如厕 2 次 · 时长正常',
          trendClass: 'trend-normal',
          badge: ''
        }
      ]
    }
  }
}
</script>

<style scoped>
.stats-container {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 20px;
  margin-bottom: 24px;
}

.stat-card {
  position: relative;
  padding: 22px 24px;
  background: linear-gradient(135deg, rgba(255, 255, 255, 0.08) 0%, rgba(255, 255, 255, 0.03) 100%);
  border-radius: 18px;
  border: 1px solid rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(15px);
  box-shadow: 
    0 8px 24px rgba(0, 0, 0, 0.2),
    inset 0 1px 0 rgba(255, 255, 255, 0.1);
  transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
  overflow: hidden;
  display: flex;
  gap: 16px;
  align-items: flex-start;
  min-height: 140px;
}

.stat-card::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  height: 3px;
  background: linear-gradient(90deg, var(--accent-color, #3b6eff), var(--accent-light, #8b5cf6));
  opacity: 0.8;
}

.stat-card:hover {
  transform: translateY(-4px);
  box-shadow: 
    0 12px 32px rgba(0, 0, 0, 0.3),
    0 0 20px var(--glow-color, rgba(59, 110, 255, 0.15));
  border-color: var(--border-color, rgba(59, 110, 255, 0.3));
}

.stat-glow {
  position: absolute;
  top: -50%;
  right: -50%;
  width: 200px;
  height: 200px;
  background: radial-gradient(circle, var(--glow-color, rgba(59, 110, 255, 0.1)) 0%, transparent 70%);
  opacity: 0;
  transition: opacity 0.4s ease;
  pointer-events: none;
}

.stat-card:hover .stat-glow {
  opacity: 1;
}

.stat-icon {
  font-size: 2.4rem;
  filter: drop-shadow(0 4px 8px rgba(0, 0, 0, 0.3));
  z-index: 1;
  transition: transform 0.3s ease;
  flex-shrink: 0;
}

.stat-card:hover .stat-icon {
  transform: scale(1.1) rotate(5deg);
}

.stat-content {
  flex: 1;
  z-index: 1;
  min-width: 0;
}

.stat-title {
  font-size: 0.82rem;
  color: rgba(234, 240, 255, 0.7);
  font-weight: 600;
  margin-bottom: 8px;
  letter-spacing: 0.5px;
  text-transform: uppercase;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.stat-value {
  font-size: 2rem;
  font-weight: 800;
  color: #ffffff;
  line-height: 1.1;
  margin-bottom: 12px;
  display: flex;
  align-items: baseline;
  gap: 6px;
}

.stat-unit {
  font-size: 0.95rem;
  font-weight: 600;
  color: rgba(234, 240, 255, 0.6);
}

.stat-trend {
  display: flex;
  align-items: center;
  gap: 8px;
  font-size: 0.78rem;
  font-weight: 500;
  padding: 6px 10px;
  border-radius: 8px;
  background: rgba(255, 255, 255, 0.03);
  width: fit-content;
  max-width: 100%;
}

.trend-warning {
  --accent-color: #ff6b6b;
  --accent-light: #ff8787;
  --glow-color: rgba(255, 107, 107, 0.15);
  --border-color: rgba(255, 107, 107, 0.3);
  color: #ff6b6b;
}

.trend-success {
  --accent-color: #51cf66;
  --accent-light: #69db7c;
  --glow-color: rgba(81, 207, 102, 0.15);
  --border-color: rgba(81, 207, 102, 0.3);
  color: #51cf66;
}

.trend-info {
  --accent-color: #339af0;
  --accent-light: #4dabf7;
  --glow-color: rgba(51, 154, 240, 0.15);
  --border-color: rgba(51, 154, 240, 0.3);
  color: #339af0;
}

.trend-normal {
  --accent-color: #ffd43b;
  --accent-light: #ffe066;
  --glow-color: rgba(255, 212, 59, 0.15);
  --border-color: rgba(255, 212, 59, 0.3);
  color: #ffd43b;
}

.trend-text {
  flex: 1;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

.trend-badge {
  padding: 2px 8px;
  background: rgba(255, 255, 255, 0.1);
  border-radius: 12px;
  font-size: 0.7rem;
  font-weight: 600;
  white-space: nowrap;
  flex-shrink: 0;
}

/* 响应式设计 */
@media (max-width: 1400px) {
  .stats-container {
    grid-template-columns: repeat(2, 1fr);
  }
}

@media (max-width: 768px) {
  .stats-container {
    grid-template-columns: 1fr;
  }
  
  .stat-card {
    padding: 20px;
  }
  
  .stat-value {
    font-size: 1.8rem;
  }
}
</style>
