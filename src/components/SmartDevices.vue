<template>
  <div class="device-card glass-card">
    <div class="device-header">
      <div class="header-left">
        <h3 class="device-title">
          <span class="title-icon">🔌</span>
          智能设备
        </h3>
        <div class="title-decoration"></div>
      </div>
      <div class="device-summary">
        <div class="summary-item online">
          <span class="status-dot pulse"></span>
          <span class="summary-label">在线</span>
          <span class="summary-value">{{ onlineCount }}</span>
        </div>
        <div class="summary-item offline">
          <span class="status-dot"></span>
          <span class="summary-label">离线</span>
          <span class="summary-value">{{ offlineCount }}</span>
        </div>
      </div>
    </div>

    <div class="device-list">
      <!-- 应急呼救类 -->
      <div class="device-category emergency">
        <h4 class="category-title">
          <span class="category-icon">🚨</span>
          应急呼救类
        </h4>
        
        <div class="device-item" v-for="device in emergencyDevices" :key="device.id">
          <div class="device-main">
            <div class="device-info">
              <div class="device-name">{{ device.name }}</div>
              <div class="device-desc">{{ device.description }}</div>
              <div class="device-location">
                <span class="location-icon">📍</span>
                {{ device.location }}
              </div>
            </div>
          </div>
          <div class="device-status" :class="device.status">
            <div class="status-indicator">
              <span class="status-icon">{{ device.status === 'online' ? '✓' : '✕' }}</span>
            </div>
            <span class="status-text">{{ device.status === 'online' ? '正常运行' : '异常' }}</span>
          </div>
        </div>
      </div>

      <!-- 风险监测类 -->
      <div class="device-category monitoring">
        <h4 class="category-title">
          <span class="category-icon">⚠️</span>
          风险监测类
        </h4>
        
        <div class="device-item" v-for="device in monitoringDevices" :key="device.id">
          <div class="device-main">
            <div class="device-info">
              <div class="device-name">{{ device.name }}</div>
              <div class="device-desc">{{ device.description }}</div>
              <div class="device-location">
                <span class="location-icon">📍</span>
                {{ device.location }}
              </div>
            </div>
          </div>
          <div class="device-status" :class="device.status">
            <div class="status-indicator">
              <span class="status-icon">{{ device.status === 'online' ? '✓' : '✕' }}</span>
            </div>
            <span class="status-text">{{ device.status === 'online' ? '正常运行' : '异常' }}</span>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'SmartDevices',
  data() {
    return {
      // 应急呼救类设备
      emergencyDevices: [
        {
          id: 1,
          name: '壁挂式一键呼救设备',
          description: '老人无需复杂操作，按下按键即可触发预警，同步接通子女、救援人员语音通话',
          location: '卧室、客厅、卫生间等老人活动高频区域',
          status: 'online'
        },
        {
          id: 2,
          name: '一键呼救手环',
          description: '手环款支持跌倒自动触发呼救，续航≥7天，支持充电提醒',
          location: '佩戴式设备',
          status: 'online'
        }
      ],
      // 风险监测类设备
      monitoringDevices: [
        {
          id: 3,
          name: '跌倒监测雷达（踢脚线人体传感器）',
          description: '1. 毫米波雷达非接触式监测，精准识别跌倒动作，区分正常弯腰、坐下与意外跌倒，误报率≤0.5%，跌倒后≤2秒触发预警，无隐私泄露风险；2. 传感器嵌入踢脚线，无感监测老人居家活动轨迹，记录作息规律（如起床、如厕、进食频率），异常状态（长时间未活动、作息突变）自动触发预警',
          location: '卧室、卫生间（跌倒高发区域）',
          status: 'online'
        },
        {
          id: 4,
          name: '燃气/烟雾报警器',
          description: '实时监测燃气浓度、烟雾浓度，超标时立即触发声光报警，同步推送预警信息至平台及相关人员，可联动燃气阀门自动关闭',
          location: '厨房（燃气使用区域）、客厅、卧室',
          status: 'online'
        }
      ]
    }
  },
  computed: {
    onlineCount() {
      const allDevices = [...this.emergencyDevices, ...this.monitoringDevices]
      return allDevices.filter(d => d.status === 'online').length
    },
    offlineCount() {
      const allDevices = [...this.emergencyDevices, ...this.monitoringDevices]
      return allDevices.filter(d => d.status !== 'online').length
    }
  }
}
</script>

<style scoped>
.device-card {
  padding: 28px;
  background: linear-gradient(135deg, rgba(255, 255, 255, 0.08) 0%, rgba(255, 255, 255, 0.03) 100%);
  border-radius: 24px;
  border: 1px solid rgba(255, 255, 255, 0.12);
  backdrop-filter: blur(20px);
  box-shadow: 
    0 12px 32px rgba(0, 0, 0, 0.25),
    inset 0 1px 0 rgba(255, 255, 255, 0.1);
  display: flex;
  flex-direction: column;
  gap: 24px;
}

/* ===== 头部区域 ===== */
.device-header {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  padding-bottom: 20px;
  border-bottom: 1px solid rgba(255, 255, 255, 0.1);
}

.header-left {
  flex: 1;
}

.device-title {
  font-size: 1.5rem;
  font-weight: 700;
  color: #ffffff;
  margin: 0 0 8px 0;
  display: flex;
  align-items: center;
  gap: 10px;
}

.title-icon {
  font-size: 1.6rem;
  filter: drop-shadow(0 2px 6px rgba(0, 0, 0, 0.3));
}

.title-decoration {
  width: 60px;
  height: 3px;
  background: linear-gradient(90deg, #5ee0b0, #26de81);
  border-radius: 2px;
}

.device-summary {
  display: flex;
  gap: 20px;
}

.summary-item {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 10px 16px;
  border-radius: 12px;
  background: rgba(255, 255, 255, 0.03);
  border: 1px solid rgba(255, 255, 255, 0.06);
  transition: all 0.3s ease;
}

.summary-item:hover {
  background: rgba(255, 255, 255, 0.06);
  transform: translateY(-2px);
}

.summary-item.online {
  border-color: rgba(46, 204, 113, 0.2);
}

.summary-item.offline {
  border-color: rgba(231, 76, 60, 0.2);
}

.status-dot {
  width: 10px;
  height: 10px;
  border-radius: 50%;
  background: #ff7b72;
  box-shadow: 0 0 8px rgba(255, 123, 114, 0.5);
}

.status-dot.pulse {
  background: #5ee0b0;
  box-shadow: 0 0 8px rgba(94, 224, 176, 0.5);
  animation: pulse 2s infinite;
}

@keyframes pulse {
  0%, 100% {
    opacity: 1;
    transform: scale(1);
  }
  50% {
    opacity: 0.7;
    transform: scale(1.2);
  }
}

.summary-label {
  font-size: 0.85rem;
  font-weight: 600;
  color: #cbd5e6;
}

.summary-value {
  font-size: 1.2rem;
  font-weight: 700;
  color: #ffffff;
}

/* ===== 设备列表 ===== */
.device-list {
  display: flex;
  flex-direction: column;
  gap: 28px;
}

.device-category {
  background: rgba(255, 255, 255, 0.03);
  border-radius: 18px;
  padding: 24px;
  border: 1px solid rgba(255, 255, 255, 0.06);
  display: flex;
  flex-direction: column;
  gap: 16px;
  transition: all 0.3s ease;
}

.device-category:hover {
  background: rgba(255, 255, 255, 0.05);
  border-color: rgba(255, 255, 255, 0.1);
}

.device-category.emergency {
  border-left: 4px solid rgba(255, 107, 107, 0.5);
}

.device-category.monitoring {
  border-left: 4px solid rgba(59, 110, 255, 0.5);
}

.category-title {
  font-size: 1.15rem;
  font-weight: 700;
  color: #ffffff;
  margin: 0 0 12px 0;
  display: flex;
  align-items: center;
  gap: 10px;
}

.category-icon {
  font-size: 1.3rem;
  filter: drop-shadow(0 2px 4px rgba(0, 0, 0, 0.2));
}

.device-item {
  background: rgba(255, 255, 255, 0.02);
  border: 1px solid rgba(255, 255, 255, 0.05);
  border-radius: 14px;
  padding: 20px;
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  gap: 20px;
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
}

.device-item:hover {
  background: rgba(255, 255, 255, 0.05);
  border-color: rgba(59, 110, 255, 0.2);
  transform: translateX(6px);
  box-shadow: 0 6px 20px rgba(0, 0, 0, 0.15);
}

.device-main {
  flex: 1;
}

.device-info {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.device-name {
  font-size: 1.05rem;
  font-weight: 600;
  color: #eef2ff;
  letter-spacing: 0.3px;
}

.device-desc {
  font-size: 0.88rem;
  color: #9fb4ff;
  line-height: 1.6;
}

.device-location {
  font-size: 0.82rem;
  color: #6c86a3;
  margin-top: 4px;
  display: flex;
  align-items: center;
  gap: 6px;
}

.location-icon {
  font-size: 0.9rem;
}

.device-status {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 8px;
  padding: 12px 18px;
  border-radius: 12px;
  min-width: 110px;
  transition: all 0.3s ease;
}

.device-status.online {
  background: rgba(46, 204, 113, 0.12);
  border: 1px solid rgba(46, 204, 113, 0.3);
}

.device-status.offline {
  background: rgba(231, 76, 60, 0.12);
  border: 1px solid rgba(231, 76, 60, 0.3);
}

.status-indicator {
  width: 36px;
  height: 36px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  background: rgba(255, 255, 255, 0.1);
}

.status-icon {
  font-size: 1.4rem;
  font-weight: bold;
}

.device-status.online .status-icon {
  color: #2ecc71;
  text-shadow: 0 0 10px rgba(46, 204, 113, 0.5);
}

.device-status.offline .status-icon {
  color: #e74c3c;
  text-shadow: 0 0 10px rgba(231, 76, 60, 0.5);
}

.status-text {
  font-size: 0.82rem;
  font-weight: 600;
}

.device-status.online .status-text {
  color: #2ecc71;
}

.device-status.offline .status-text {
  color: #e74c3c;
}

/* 响应式设计 */
@media (max-width: 768px) {
  .device-card {
    padding: 20px;
  }

  .device-header {
    flex-direction: column;
    gap: 16px;
  }

  .device-summary {
    width: 100%;
    justify-content: space-between;
  }

  .device-item {
    flex-direction: column;
    gap: 16px;
  }

  .device-status {
    flex-direction: row;
    justify-content: space-between;
    width: 100%;
    min-width: auto;
  }
}
</style>