<template>
  <div class="device-card glass-card">
    <div class="device-header">
      <h3 class="device-title">智能设备</h3>
      <div class="device-summary">
        <span class="summary-item">
          <span class="dot green-dot"></span>
          在线: {{ onlineCount }}
        </span>
        <span class="summary-item">
          <span class="dot red-dot"></span>
          离线: {{ offlineCount }}
        </span>
      </div>
    </div>

    <div class="device-list">
      <!-- 应急呼救类 -->
      <div class="device-category">
        <h4 class="category-title">🚨 应急呼救类</h4>
        
        <!-- 壁挂式一键呼救设备 -->
        <div class="device-item" v-for="device in emergencyDevices" :key="device.id">
          <div class="device-info">
            <div class="device-name">{{ device.name }}</div>
            <div class="device-desc">{{ device.description }}</div>
            <div class="device-location">📍 {{ device.location }}</div>
          </div>
          <div class="device-status" :class="device.status">
            <span class="status-icon">{{ device.status === 'online' ? '✓' : '✕' }}</span>
            <span class="status-text">{{ device.status === 'online' ? '正常运行' : '异常' }}</span>
          </div>
        </div>
      </div>

      <!-- 风险监测类 -->
      <div class="device-category">
        <h4 class="category-title">⚠️ 风险监测类</h4>
        
        <div class="device-item" v-for="device in monitoringDevices" :key="device.id">
          <div class="device-info">
            <div class="device-name">{{ device.name }}</div>
            <div class="device-desc">{{ device.description }}</div>
            <div class="device-location">📍 {{ device.location }}</div>
          </div>
          <div class="device-status" :class="device.status">
            <span class="status-icon">{{ device.status === 'online' ? '✓' : '✕' }}</span>
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
  padding: 24px;
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.device-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding-bottom: 16px;
  border-bottom: 1px solid rgba(255, 255, 255, 0.1);
}

.device-title {
  font-size: 1.3rem;
  color: #dfe8ff;
  font-weight: 600;
  margin: 0;
}

.device-summary {
  display: flex;
  gap: 16px;
}

.summary-item {
  display: flex;
  align-items: center;
  gap: 6px;
  font-size: 0.85rem;
  color: #9fb4ff;
}

.dot {
  width: 8px;
  height: 8px;
  border-radius: 50%;
}

.green-dot {
  background: #2ecc71;
  box-shadow: 0 0 6px #2ecc71;
}

.red-dot {
  background: #e74c3c;
  box-shadow: 0 0 6px #e74c3c;
}

.device-list {
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.device-category {
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.category-title {
  font-size: 1rem;
  color: #b9c8ff;
  font-weight: 600;
  margin: 0 0 8px 0;
}

.device-item {
  background: rgba(255, 255, 255, 0.03);
  border: 1px solid rgba(72, 120, 255, 0.15);
  border-radius: 12px;
  padding: 16px;
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  gap: 16px;
  transition: all 0.2s ease;
}

.device-item:hover {
  background: rgba(255, 255, 255, 0.05);
  border-color: rgba(72, 120, 255, 0.3);
}

.device-info {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.device-name {
  font-size: 1rem;
  font-weight: 600;
  color: #eef2ff;
}

.device-desc {
  font-size: 0.85rem;
  color: #9fb4ff;
  line-height: 1.5;
}

.device-location {
  font-size: 0.8rem;
  color: #6c86a3;
  margin-top: 4px;
}

.device-status {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 6px;
  padding: 10px 16px;
  border-radius: 8px;
  min-width: 100px;
}

.device-status.online {
  background: rgba(46, 204, 113, 0.15);
  border: 1px solid rgba(46, 204, 113, 0.3);
}

.device-status.offline {
  background: rgba(231, 76, 60, 0.15);
  border: 1px solid rgba(231, 76, 60, 0.3);
}

.status-icon {
  font-size: 1.5rem;
  font-weight: bold;
}

.device-status.online .status-icon {
  color: #2ecc71;
}

.device-status.offline .status-icon {
  color: #e74c3c;
}

.status-text {
  font-size: 0.8rem;
  font-weight: 500;
}

.device-status.online .status-text {
  color: #2ecc71;
}

.device-status.offline .status-text {
  color: #e74c3c;
}
</style>