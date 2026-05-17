<template>
  <div class="admin-page">
    <div class="admin-head">
      <div class="admin-title-group">
        <div class="logo-wrapper">
          <div class="logo">安芯居</div>
          <div class="logo-ring"></div>
        </div>
        <div>
          <p class="subtitle">安芯居家庭守护系统</p>
          <h1>管理员控制台</h1>
          <p class="description">集中展示家庭守护系统报警、健康趋势与用户推送消息。</p>
        </div>
      </div>
      <button class="btn-logout" @click="handleLogout">退出登录</button>
    </div>

    <div class="overview-grid">
      <div class="metric-card">
        <p class="metric-label">降低跌倒致死率</p>
        <p class="metric-value">60% 以上</p>
      </div>
      <div class="metric-card">
        <p class="metric-label">拯救严重安全事故</p>
        <p class="metric-value">15-20 起</p>
      </div>
      <div class="metric-card">
        <p class="metric-label">节约家庭照护时长</p>
        <p class="metric-value">30-45 万小时</p>
      </div>
      <div class="metric-card">
        <p class="metric-label">用户满意度</p>
        <p class="metric-value">95% 以上</p>
      </div>
      <div class="metric-card">
        <p class="metric-label">预警响应时间</p>
        <p class="metric-value">≤ 5 分钟</p>
      </div>
      <div class="metric-card">
        <p class="metric-label">设备正常运行率</p>
        <p class="metric-value">≥ 98%</p>
      </div>
      <div class="metric-card">
        <p class="metric-label">算法误报率</p>
        <p class="metric-value">≤ 0.5%</p>
      </div>
      <div class="metric-card">
        <p class="metric-label">志愿者参与</p>
        <p class="metric-value">200+ 人次</p>
      </div>
    </div>

    <div class="main-panel">
      <section class="glass-card message-panel">
        <div class="panel-header">
          <div>
            <h2>实时消息推送</h2>
            <p>系统预置 10 条老年人关怀消息，按条轮换显示，确保每条推送都能被及时查看。</p>
          </div>
          <span class="status-tag">最新消息</span>
        </div>

        <div class="message-scroll" ref="messageScroll">
          <ul class="message-list">
            <li
              v-for="(message, index) in messages"
              :key="index"
              :ref="'messageItems'"
              :class="['message-item', { active: index === currentIndex }]">
              <div class="message-top">
                <div class="message-type" :class="message.typeClass">{{ message.title }}</div>
                <div class="message-time">{{ message.time }}</div>
              </div>
              <div class="message-content">
                <div class="message-user">{{ message.user }}</div>
                <div class="message-text">{{ message.text }}</div>
              </div>
            </li>
          </ul>
        </div>
      </section>

      <section class="glass-card log-panel">
        <div class="panel-header">
          <div>
            <h2>系统调度实况</h2>
            <p>展示当前家庭守护预警、传感器记录与智能调度日志，按时间竖向轮播播放。</p>
          </div>
          <span class="status-tag status-live">运行中</span>
        </div>

        <div class="log-scroll" ref="logScroll">
          <div class="log-list">
            <div
              v-for="(log, index) in logs"
              :key="index"
              :ref="'logItems'"
              :class="['log-entry', { active: index === logCurrentIndex }]">
              <span>{{ log.time }}</span>
              <div class="log-body">{{ log.text }}</div>
            </div>
          </div>
        </div>

        <div class="log-dots">
          <span
            v-for="(log, index) in logs"
            :key="index"
            class="dot"
            :class="{ active: index === logCurrentIndex }"
            @click="setCurrentLog(index)">
          </span>
        </div>
      </section>
    </div>
  </div>
</template>

<script>
export default {
  name: 'AdminPage',
  props: {
    currentUser: {
      type: String,
      default: '管理员'
    }
  },
  data() {
    return {
      currentIndex: 0,
      logCurrentIndex: 0,
      messages: [
        {
          title: '摔倒检测',
          typeClass: 'message-fall',
          user: '刘奶奶 · 客厅',
          text: '检测到摔倒风险，建议立即派发语音提醒并通知家属。',
          time: '14:38:12'
        },
        {
          title: '行为异常',
          typeClass: 'message-abnormal',
          user: '张爷爷 · 室内走动',
          text: '夜间活动异常增加，系统判定行为趋势异常。',
          time: '14:24:05'
        },
        {
          title: '智能警报',
          typeClass: 'message-alert',
          user: '王女士 · 厨房',
          text: '烟雾探测器触发报警，建议检查厨房安全并通知巡检人员。',
          time: '13:57:33'
        },
        {
          title: '健康提醒',
          typeClass: 'message-health',
          user: '陈先生 · 卧室',
          text: '心率波动异常，已生成健康报告并建议安排复查。',
          time: '13:34:20'
        },
        {
          title: '跌倒预警',
          typeClass: 'message-fall',
          user: '李大爷 · 客厅',
          text: '检测到疑似跌倒动作，建议马上进行语音确认。',
          time: '13:10:45'
        },
        {
          title: '行动迟缓',
          typeClass: 'message-abnormal',
          user: '黄奶奶 · 卧室',
          text: '活动速度明显下降，系统建议提醒佩戴者休息并检查血压。',
          time: '12:55:18'
        },
        {
          title: '烟雾警报',
          typeClass: 'message-alert',
          user: '赵先生 · 厨房',
          text: '厨房烟雾浓度升高，已触发智能预警并建议关闭燃气。',
          time: '12:40:06'
        },
        {
          title: '异常心率',
          typeClass: 'message-health',
          user: '王奶奶 · 客厅',
          text: '心率突然升高，系统已经生成报警并通知健康中心。',
          time: '12:20:54'
        },
        {
          title: '夜间异常',
          typeClass: 'message-abnormal',
          user: '陈大爷 · 走廊',
          text: '夜间活动轨迹偏离常规路线，建议巡查是否需要帮助。',
          time: '11:58:30'
        },
        {
          title: '紧急提醒',
          typeClass: 'message-alert',
          user: '孙女士 · 卧室',
          text: '传感器检测到异常震动，已启动智能警报并联系值守人员。',
          time: '11:25:12'
        }
      ],
      logs: [
        { time: '14:25:00', text: '如厕 夜间第2次如厕，时长正常' },
        { time: '14:20:44', text: '安全 摔倒风险未触发，姿态稳定' },
        { time: '14:15:32', text: '空气质量 PM2.5瞬时值42，建议开启净化' },
        { time: '14:38:12', text: '心率异常分析：夜间恢复指数偏低' },
        { time: '14:36:47', text: '路径图 偏离卧室-卫生间常规动线，已预警' },
        { time: '14:34:02', text: '空气质量传感器 自动联动新风系统，已执行' },
        { time: '14:33:20', text: '心电图设备 更新导联灵敏度，重新校准' },
        { time: '14:30:10', text: '环境监测 厨房温度异常，已调整空调模式。' },
        { time: '14:28:55', text: '门禁 玄关门锁已自动复位，当前状态安全。' },
        { time: '14:26:41', text: '药物提醒 李爷爷已按时服药，系统记录完成。' },
        { time: '14:22:18', text: '异常活动 卧室门频繁开启，建议检查是否需要帮助。' },
        { time: '14:18:05', text: '健康监测 体温略高，已提醒管理员关注。' }
      ],
      intervalId: null
    }
  },
  computed: {
    currentMessage() {
      return this.messages[this.currentIndex] || this.messages[0]
    },
    currentLog() {
      return this.logs[this.logCurrentIndex] || this.logs[0]
    }
  },
  mounted() {
    this.intervalId = setInterval(() => {
      this.nextMessage()
      this.nextLog()
    }, 4500)
    this.$nextTick(() => {
      this.scrollActiveMessage()
      this.scrollActiveLog()
    })
  },
  beforeUnmount() {
    if (this.intervalId) {
      clearInterval(this.intervalId)
    }
  },
  methods: {
    handleLogout() {
      this.$emit('logout')
    },
    nextMessage() {
      this.currentIndex = (this.currentIndex + 1) % this.messages.length
      this.$nextTick(this.scrollActiveMessage)
    },
    nextLog() {
      this.logCurrentIndex = (this.logCurrentIndex + 1) % this.logs.length
      this.$nextTick(this.scrollActiveLog)
    },
    setCurrentMessage(index) {
      this.currentIndex = index
      this.$nextTick(this.scrollActiveMessage)
    },
    setCurrentLog(index) {
      this.logCurrentIndex = index
      this.$nextTick(this.scrollActiveLog)
    },
    scrollActiveMessage() {
      const items = this.$refs.messageItems
      const container = this.$refs.messageScroll
      const active = Array.isArray(items) ? items[this.currentIndex] : null
      if (active && container) {
        const activeRect = active.getBoundingClientRect()
        const containerRect = container.getBoundingClientRect()
        container.scrollTop += activeRect.top - containerRect.top
      }
    },
    scrollActiveLog() {
      const items = this.$refs.logItems
      const container = this.$refs.logScroll
      const active = Array.isArray(items) ? items[this.logCurrentIndex] : null
      if (active && container) {
        const activeRect = active.getBoundingClientRect()
        const containerRect = container.getBoundingClientRect()
        container.scrollTop += activeRect.top - containerRect.top
      }
    }
  }
}
</script>

<style scoped>
.admin-page {
  width: 100%;
  max-width: 1500px;
  margin: 0 auto;
  padding: 28px 32px;
  display: flex;
  flex-direction: column;
  gap: 24px;
}

.admin-head {
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 20px;
  background: rgba(18, 25, 45, 0.84);
  border: 1px solid rgba(72, 120, 255, 0.24);
  border-radius: 24px;
  padding: 24px 28px;
  backdrop-filter: blur(18px);
}

.admin-title-group {
  display: flex;
  align-items: center;
  gap: 18px;
}

.logo-wrapper {
  position: relative;
  width: 68px;
  height: 68px;
  border-radius: 20px;
  background: rgba(255, 255, 255, 0.08);
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow: inset 0 0 0 1px rgba(255, 255, 255, 0.08);
}

.logo {
  color: #eef2ff;
  font-size: 1.05rem;
  font-weight: 700;
  letter-spacing: 1px;
}

.logo-ring {
  position: absolute;
  inset: 0;
  border: 1px solid rgba(94, 150, 255, 0.35);
  border-radius: 20px;
}

.subtitle {
  color: #8db7ff;
  font-size: 0.95rem;
  margin-bottom: 8px;
}

.admin-head h1 {
  font-size: 2rem;
  color: #f7faff;
  margin-bottom: 8px;
}

.description {
  color: #c9d6ff;
  opacity: 0.92;
  max-width: 640px;
  line-height: 1.7;
}

.btn-logout {
  background: rgba(255, 123, 114, 0.16);
  border: 1px solid rgba(255, 123, 114, 0.35);
  color: #ffb4aa;
  padding: 14px 28px;
  border-radius: 26px;
  cursor: pointer;
  font-weight: 600;
  transition: all 0.25s ease;
}

.btn-logout:hover {
  background: rgba(255, 123, 114, 0.32);
  color: #fff;
  transform: translateY(-1px);
}

.overview-grid {
  display: grid;
  grid-template-columns: repeat(4, minmax(0, 1fr));
  gap: 20px;
}

.metric-card {
  padding: 24px;
  border-radius: 22px;
  background: rgba(15, 24, 50, 0.78);
  border: 1px solid rgba(255, 255, 255, 0.08);
  box-shadow: 0 20px 50px rgba(0, 0, 0, 0.16);
}

.metric-label {
  color: #b9c8ff;
  font-size: 0.95rem;
  margin-bottom: 10px;
}

.metric-value {
  color: #f6fbff;
  font-size: 1.9rem;
  font-weight: 700;
}

.main-panel {
  display: grid;
  grid-template-columns: 1.6fr 1fr;
  gap: 24px;
}

.glass-card {
  padding: 26px;
  border-radius: 24px;
  background: rgba(15, 24, 50, 0.72);
  border: 1px solid rgba(255, 255, 255, 0.08);
  backdrop-filter: blur(20px);
  box-shadow: 0 24px 80px rgba(0, 0, 0, 0.18);
}

.panel-header {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  gap: 16px;
  margin-bottom: 22px;
}

.panel-header h2 {
  color: #fff;
  font-size: 1.4rem;
  margin-bottom: 8px;
}

.panel-header p {
  color: #c9d6ff;
  opacity: 0.94;
  line-height: 1.7;
}

.message-carousel {
  display: flex;
  flex-direction: column;
  gap: 18px;
}

.message-item {
  display: flex;
  flex-direction: column;
  gap: 16px;
  padding: 24px 26px;
  background: rgba(8, 18, 40, 0.78);
  border: 1px solid rgba(94, 150, 255, 0.16);
  border-radius: 22px;
}

.message-scroll,
.log-scroll {
  max-height: 520px;
  overflow-y: auto;
  padding-right: 8px;
    padding-bottom: 12px;
    display: flex;
    flex-direction: column;
    gap: 14px;
    margin: 0;
    padding: 0;
    list-style: none;
  }

  .log-list {
    padding: 0;
  background: rgba(8, 18, 40, 0.78);
  border: 1px solid rgba(94, 150, 255, 0.16);
}

.message-item.active {
  border-color: rgba(79, 157, 255, 0.6);
  box-shadow: 0 0 0 1px rgba(79, 157, 255, 0.18);
}

.message-top {
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 12px;
}

.message-type {
  padding: 12px 16px;
  border-radius: 18px;
  text-align: center;
  font-weight: 700;
  color: #eef2ff;
  font-size: 1rem;
}

.message-content {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.message-user {
  font-weight: 700;
  color: #f8fbff;
  font-size: 1.05rem;
}

.message-text {
  color: #e4ecff;
  opacity: 0.98;
  line-height: 1.8;
  font-size: 1rem;
}

.message-time {
  color: #9eb6ff;
  text-align: right;
  font-size: 0.98rem;
}

.dot {
  width: 12px;
  height: 12px;
  border-radius: 999px;
  background: rgba(255, 255, 255, 0.24);
  cursor: pointer;
  transition: transform 0.2s ease, background 0.2s ease;
}

.dot.active {
  background: #4f9dff;
  transform: scale(1.2);
}

@media (max-width: 960px) {
  .message-item {
    text-align: left;
  }

  .message-time {
    text-align: left;
  }
}

.status-tag {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  padding: 10px 16px;
  border-radius: 999px;
  font-size: 0.88rem;
  font-weight: 700;
  color: #fff;
  background: rgba(56, 189, 248, 0.16);
  border: 1px solid rgba(56, 189, 248, 0.24);
}

.status-live {
  background: rgba(34, 197, 94, 0.16);
  border-color: rgba(34, 197, 94, 0.24);
}

.log-carousel {
  display: flex;
  flex-direction: column;
  gap: 18px;
}

.log-card {
  padding: 24px 26px;
  border-radius: 22px;
  background: rgba(8, 18, 40, 0.78);
  border: 1px solid rgba(94, 150, 255, 0.16);
  box-shadow: inset 0 0 0 1px rgba(94, 150, 255, 0.06);
}

.log-time {
  color: #9eb6ff;
  font-weight: 700;
  margin-bottom: 10px;
}

.log-body {
  color: #e4ecff;
  line-height: 1.8;
  font-size: 1rem;
}

.log-dots {
  display: flex;
  gap: 10px;
  justify-content: center;
}

@media (max-width: 1240px) {
  .overview-grid {
    grid-template-columns: repeat(2, minmax(0, 1fr));
  }

  .main-panel {
    grid-template-columns: 1fr;
  }
}

@media (max-width: 760px) {
  .admin-head,
  .message-item,
  .log-entry {
    flex-direction: column;
    align-items: stretch;
  }

  .message-item {
    grid-template-columns: 1fr;
  }

  .message-time {
    text-align: left;
  }
}
</style>
