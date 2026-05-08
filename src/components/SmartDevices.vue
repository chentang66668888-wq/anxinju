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
  padding: 25px;
  background: rgba(255, 255, 255, 0.08);
  border-radius: 20px;
  border: 1px solid rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(15px);
  box-shadow: 0 8px 25px rgba(0, 0, 0, 0.2);
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.device-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding-bottom: 15px;
  border-bottom: 1px solid rgba(255, 255, 255, 0.1);
}

.device-title {
  font-size: 1.4rem;
  font-weight: 700;
  color: #ffffff;
  margin: 0;
  position: relative;
}

.device-title::after {
  content: '';
  position: absolute;
  bottom: -5px;
  left: 0;
  width: 40px;
  height: 3px;
  background: linear-gradient(90deg, #5ee0b0, #26de81);
  border-radius: 2px;
}

.device-summary {
  display: flex;
  gap: 15px;
}

.summary-item {
  display: flex;
  align-items: center;
  gap: 8px;
  font-size: 0.85rem;
  font-weight: 500;
  color: #cbd5e6;
}

.dot {
  width: 8px;
  height: 8px;
  border-radius: 50%;
}

.green-dot {
  background: #5ee0b0;
  box-shadow: 0 0 6px rgba(94, 224, 176, 0.5);
}

.red-dot {
  background: #ff7b72;
  box-shadow: 0 0 6px rgba(255, 123, 114, 0.5);
}

.device-list {
  display: flex;
  flex-direction: column;
  gap: 25px;
}

.device-category {
  background: rgba(255, 255, 255, 0.03);
  border-radius: 15px;
  padding: 20px;
  border: 1px solid rgba(255, 255, 255, 0.05);
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.category-title {
  font-size: 1.1rem;
  font-weight: 700;
  color: #ffffff;
  margin: 0 0 8px 0;
  display: flex;
  align-items: center;
  gap: 8px;
}

.category-title::before {
  content: '';
  width: 4px;
  height: 20px;
  background: linear-gradient(180deg, #3b6eff, #8b5cf6);
  border-radius: 2px;
}

.device-item {
  background: rgba(255, 255, 255, 0.02);
  border: 1px solid rgba(255, 255, 255, 0.03);
  border-radius: 12px;
  padding: 16px;
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  gap: 16px;
  transition: all 0.3s ease;
}

.device-item:hover {
  background: rgba(255, 255, 255, 0.05);
  border-color: rgba(59, 110, 255, 0.2);
  transform: translateY(-2px);
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.1);
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

/* Responsive design */
@media (max-width: 768px) {
  .device-card {
    padding: 20px;
  }

  .device-header {
    flex-direction: column;
    gap: 15px;
    align-items: flex-start;
  }

  .device-summary {
    width: 100%;
    justify-content: space-between;
  }

  .device-item {
    flex-direction: column;
    gap: 12px;
  }

  .device-info {
    margin-right: 0;
  }

  .device-status {
    flex-direction: row;
    justify-content: space-between;
    width: 100%;
    min-width: auto;
  }
}
</style>