<template>
  <div class="status-bar">
    <div class="status-left">
      <div class="status-item" v-for="(item, index) in statusItems" :key="index">
        <span :class="['status-dot', item.dotClass]"></span>
        <span class="status-text">{{ item.text }}</span>
      </div>
    </div>
    <div class="sync-time">
      <span class="time-icon">🕒</span>
      <span>同步时间：{{ currentTime }} &nbsp;|&nbsp; 成功拉取高新区居家养老数据</span>
    </div>
  </div>
</template>

<script>
export default { 
  name: 'StatusBar',
  data() {
    return {
      currentTime: '2026-04-21 14:35:22',
      statusItems: [
        { text: '本地数据库：已连接', dotClass: 'green-dot' },
        { text: '毫米波雷达：在线', dotClass: 'green-dot' },
        { text: '心电图传感器：正常', dotClass: 'green-dot' },
        { text: '路径图引擎：活跃', dotClass: 'blue-dot' }
      ]
    }
  }
}
</script>

<style scoped>
.status-bar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 16px 24px;
  background: linear-gradient(135deg, rgba(255, 255, 255, 0.06) 0%, rgba(255, 255, 255, 0.02) 100%);
  border-radius: 16px;
  border: 1px solid rgba(255, 255, 255, 0.08);
  backdrop-filter: blur(10px);
  box-shadow: 0 4px 16px rgba(0, 0, 0, 0.15);
  gap: 20px;
}

.status-left {
  display: flex;
  gap: 20px;
  flex-wrap: wrap;
}

.status-item {
  display: flex;
  align-items: center;
  gap: 8px;
  padding: 8px 14px;
  background: rgba(255, 255, 255, 0.03);
  border-radius: 10px;
  border: 1px solid rgba(255, 255, 255, 0.05);
  transition: all 0.3s ease;
}

.status-item:hover {
  background: rgba(255, 255, 255, 0.06);
  border-color: rgba(255, 255, 255, 0.1);
  transform: translateY(-2px);
}

.status-dot {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  position: relative;
}

.green-dot {
  background: #5ee0b0;
  box-shadow: 0 0 8px rgba(94, 224, 176, 0.6);
  animation: pulse-green 2s infinite;
}

.blue-dot {
  background: #3b6eff;
  box-shadow: 0 0 8px rgba(59, 110, 255, 0.6);
  animation: pulse-blue 2s infinite;
}

@keyframes pulse-green {
  0%, 100% {
    opacity: 1;
    transform: scale(1);
  }
  50% {
    opacity: 0.7;
    transform: scale(1.2);
  }
}

@keyframes pulse-blue {
  0%, 100% {
    opacity: 1;
    transform: scale(1);
  }
  50% {
    opacity: 0.7;
    transform: scale(1.2);
  }
}

.status-text {
  font-size: 0.85rem;
  color: #cbd5e6;
  font-weight: 500;
  white-space: nowrap;
}

.sync-time {
  display: flex;
  align-items: center;
  gap: 8px;
  padding: 8px 14px;
  background: rgba(59, 110, 255, 0.08);
  border-radius: 10px;
  border: 1px solid rgba(59, 110, 255, 0.15);
  font-size: 0.85rem;
  color: #a8c7ff;
  font-weight: 500;
  white-space: nowrap;
}

.time-icon {
  font-size: 1rem;
}

/* 响应式设计 */
@media (max-width: 1200px) {
  .status-bar {
    flex-direction: column;
    gap: 12px;
  }
  
  .status-left {
    width: 100%;
    justify-content: center;
  }
  
  .sync-time {
    width: 100%;
    justify-content: center;
  }
}

@media (max-width: 768px) {
  .status-bar {
    padding: 12px 16px;
  }
  
  .status-left {
    gap: 10px;
  }
  
  .status-item {
    padding: 6px 10px;
  }
  
  .status-text {
    font-size: 0.75rem;
  }
}
</style>
