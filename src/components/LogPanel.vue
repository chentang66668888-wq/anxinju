<template>
  <div class="log-panel glass-card">
    <div class="log-header">📡 系统调度实况 · 实时监测日志</div>
    <div class="log-list" ref="logContainer">
      <div v-for="(log, idx) in logs" :key="idx" class="log-item">
        <span class="log-time">{{ log.time }}</span>
        <span class="log-text">{{ log.text }} <span v-if="log.badge" class="log-badge">{{ log.badge }}</span></span>
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
        { time: '[14:30:22]', text: '心电图 心率偏快(98bpm) 已提醒家属', badge: '' },
        { time: '[14:28:15]', text: '路径图 今日活动路径偏离日常轨迹30%', badge: '' },
        { time: '[14:25:00]', text: '如厕 夜间第2次如厕，时长正常', badge: '' },
        { time: '[14:20:44]', text: '安全 摔倒风险未触发 · 姿态稳定', badge: '' },
        { time: '[14:15:32]', text: '空气质量 PM2.5瞬时值42，建议开启净化', badge: '' }
      ],
      extraLogs: [
        { text: '心率变异性分析: 夜间恢复指数偏低', badge: '建议' },
        { text: '路径图 偏离卧室-卫生间常规动线，已标记', badge: '' },
        { text: '空气质量传感器 自动联动新风系统', badge: '已执行' },
        { text: '心电图设置 更新导联灵敏度，重新校准', badge: '' }
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
    getCurrentSimTime() { const d = new Date(); return `[${String(d.getHours()).padStart(2,'0')}:${String(d.getMinutes()).padStart(2,'0')}:${String(d.getSeconds()).padStart(2,'0')}]` },
    scrollToEnd() { const c = this.$refs.logContainer; if (c) c.scrollTop = c.scrollHeight }
  }
}
</script>

<style scoped>
/* log styles are in Dashboard.vue, keep local tweaks minimal */
</style>
