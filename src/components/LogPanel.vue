<template>
  <div class="log-panel glass-card">
    <div class="log-header">
      <span class="header-icon">📡</span>
      <span class="header-title">系统调度实况 · 实时监测日志</span>
      <div class="header-decoration"></div>
    </div>
    <div class="log-list" ref="logContainer">
      <div v-for="(log, idx) in logs" :key="idx" class="log-item" :class="getLogClass(log)">
        <span class="log-time">{{ log.time }}</span>
        <span class="log-text">{{ log.text }}</span>
        <span v-if="log.badge" :class="['log-badge', getBadgeClass(log.badge)]">{{ log.badge }}</span>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'LogPanel',
  data() {
    return {
      logs: [
        { time: '[14:32:01]', text: '检测 客厅异常停留 >10分钟', badge: '关注' },
        { time: '[14:30:22]', text: '心率图 心率偏快(98bpm) 已提醒家属', badge: '' },
        { time: '[14:28:15]', text: '路径图 今日活动路径偏离日常轨迹30%', badge: '' },
        { time: '[14:25:00]', text: '如厕 夜间第2次如厕，时长正常', badge: '' },
        { time: '[14:20:44]', text: '安全 摔倒风险未触发 · 姿态稳定', badge: '' },
        { time: '[14:15:32]', text: '空气质量 PM2.5瞬时值42，建议开启净化', badge: '' }
      ],
      extraLogs: [
        { text: '心率变异性分析: 夜间恢复指数偏低', badge: '建议' },
        { text: '路径图 偏离卧室-卫生间常规动线，已标记', badge: '' },
        { text: '空气质量传感器 自动联动新风系统', badge: '已执行' },
        { text: '心率图设置 更新导联灵敏度，重新校准', badge: '' }
      ],
      intervalId: null,
      intervalCount: 0
    }
  },
  mounted() {
    // 延迟追加一些日志
    setTimeout(() => {
      this.extraLogs.forEach((l) => this.logs.push({ time: this.getCurrentSimTime(), text: l.text, badge: l.badge }))
      this.scrollToEnd()
    }, 800)

    // 模拟几次实时事件
    this.intervalId = setInterval(() => {
      this.intervalCount++
      if (this.intervalCount > 3) { clearInterval(this.intervalId); return }
      const ev = this.intervalCount % 2 === 0
        ? { text: '毫米波雷达检测到起身动作，如厕计时开始', badge: '实时' }
        : { text: '算法检测夜间体温略高，建议关注', badge: '风险' }
      this.logs.push({ time: this.getCurrentSimTime(), text: ev.text, badge: ev.badge })
      if (this.logs.length > 14) this.logs.shift()
      this.$nextTick(() => this.scrollToEnd())
    }, 4200)
  },
  beforeUnmount() { if (this.intervalId) clearInterval(this.intervalId) },
  methods: {
    getCurrentSimTime() { 
      const d = new Date(); 
      return `[${String(d.getHours()).padStart(2,'0')}:${String(d.getMinutes()).padStart(2,'0')}:${String(d.getSeconds()).padStart(2,'0')}]` 
    },
    scrollToEnd() { 
      const c = this.$refs.logContainer; 
      if (c) c.scrollTop = c.scrollHeight 
    },
    getLogClass(log) {
      if (log.badge === '风险') return 'risk-log'
      if (log.badge === '关注') return 'attention-log'
      if (log.badge === '实时') return 'realtime-log'
      return ''
    },
    getBadgeClass(badge) {
      if (badge === '风险') return 'badge-risk'
      if (badge === '关注') return 'badge-attention'
      if (badge === '实时') return 'badge-realtime'
      if (badge === '建议') return 'badge-suggestion'
      if (badge === '已执行') return 'badge-executed'
      return ''
    }
  }
}
</script>

<style scoped>
.log-panel {
  padding: 24px;
  background: linear-gradient(135deg, rgba(255, 255, 255, 0.06) 0%, rgba(255, 255, 255, 0.02) 100%);
  border-radius: 20px;
  border: 1px solid rgba(255, 255, 255, 0.08);
  backdrop-filter: blur(15px);
  box-shadow: 
    0 8px 24px rgba(0, 0, 0, 0.2),
    inset 0 1px 0 rgba(255, 255, 255, 0.05);
  display: flex;
  flex-direction: column;
  gap: 20px;
}

/* ===== 头部区域 ===== */
.log-header {
  display: flex;
  align-items: center;
  gap: 12px;
  padding-bottom: 16px;
  border-bottom: 1px solid rgba(255, 255, 255, 0.1);
  position: relative;
}

.header-icon {
  font-size: 1.4rem;
  filter: drop-shadow(0 2px 4px rgba(0, 0, 0, 0.2));
}

.header-title {
  font-size: 1.1rem;
  font-weight: 700;
  color: #dfe8ff;
  letter-spacing: 0.5px;
}

.header-decoration {
  position: absolute;
  bottom: -1px;
  left: 0;
  width: 80px;
  height: 2px;
  background: linear-gradient(90deg, #3b6eff, #8b5cf6);
  border-radius: 2px;
}

/* ===== 日志列表 ===== */
.log-list {
  display: flex;
  flex-direction: column;
  gap: 12px;
  max-height: 400px;
  overflow-y: auto;
  padding-right: 8px;
}

.log-list::-webkit-scrollbar {
  width: 6px;
}

.log-list::-webkit-scrollbar-track {
  background: rgba(255, 255, 255, 0.02);
  border-radius: 3px;
}

.log-list::-webkit-scrollbar-thumb {
  background: rgba(59, 110, 255, 0.3);
  border-radius: 3px;
}

.log-list::-webkit-scrollbar-thumb:hover {
  background: rgba(59, 110, 255, 0.5);
}

.log-item {
  display: flex;
  align-items: flex-start;
  gap: 12px;
  padding: 14px 16px;
  background: rgba(255, 255, 255, 0.02);
  border-radius: 12px;
  border-left: 3px solid transparent;
  transition: all 0.3s ease;
  animation: slideIn 0.3s ease;
}

@keyframes slideIn {
  from {
    opacity: 0;
    transform: translateX(-10px);
  }
  to {
    opacity: 1;
    transform: translateX(0);
  }
}

.log-item:hover {
  background: rgba(255, 255, 255, 0.04);
  transform: translateX(4px);
}

.risk-log {
  border-left-color: #ff6b6b;
  background: rgba(255, 107, 107, 0.05);
}

.attention-log {
  border-left-color: #ffd43b;
  background: rgba(255, 212, 59, 0.05);
}

.realtime-log {
  border-left-color: #3b6eff;
  background: rgba(59, 110, 255, 0.05);
}

.log-time {
  font-size: 0.82rem;
  color: #6c86a3;
  font-weight: 600;
  white-space: nowrap;
  min-width: 85px;
}

.log-text {
  flex: 1;
  font-size: 0.9rem;
  color: #cbd5e6;
  line-height: 1.5;
}

.log-badge {
  padding: 4px 10px;
  border-radius: 12px;
  font-size: 0.75rem;
  font-weight: 600;
  white-space: nowrap;
}

.badge-risk {
  background: rgba(255, 107, 107, 0.15);
  color: #ff6b6b;
  border: 1px solid rgba(255, 107, 107, 0.3);
}

.badge-attention {
  background: rgba(255, 212, 59, 0.15);
  color: #ffd43b;
  border: 1px solid rgba(255, 212, 59, 0.3);
}

.badge-realtime {
  background: rgba(59, 110, 255, 0.15);
  color: #3b6eff;
  border: 1px solid rgba(59, 110, 255, 0.3);
}

.badge-suggestion {
  background: rgba(94, 224, 176, 0.15);
  color: #5ee0b0;
  border: 1px solid rgba(94, 224, 176, 0.3);
}

.badge-executed {
  background: rgba(139, 92, 246, 0.15);
  color: #8b5cf6;
  border: 1px solid rgba(139, 92, 246, 0.3);
}
</style>
