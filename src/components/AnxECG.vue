<template>
  <div class="ecg-card glass-card">
    <div class="ecg-title">心电图</div>
    <div class="ecg-plot" :style="{ background: getBgColor() }">
      <canvas ref="canvas" class="ecg-canvas"></canvas>
      <div v-if="ecgSettings.showData" class="ecg-data-display">
        <div class="data-item">心率: {{ heartRate }} BPM</div>
        <div class="data-item">电压: {{ currentVoltage }} mV</div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'AnxECG',
  props: {
    speed: { type: Number, default: 80 },
    ecgSettings: {
      type: Object,
      default: () => ({
        bgColor: 'dark',
        showData: false,
        waveColor: 'lightblue'
      })
    }
  },
  data() {
    return {
      ctx: null,
      width: 0,
      height: 0,
      dpr: 1,
      pattern: null,
      gridCanvas: null,
      resizeTimer: null,
      csvValues: null,
      animationId: null,
      offset: 0,
      lastTime: 0,
      regenTimer: null,
      heartRate: 72,
      currentVoltage: 0
    }
  },
  watch: {
    ecgSettings: {
      deep: true,
      handler() {
        this.updateDisplay()
      }
    }
  },
  mounted() {
    this.setupCanvas()
    this.loadCSV().then(ok => {
      if (ok) {
        this.pattern = this.createPatternFromCSV(this.csvValues.length)
      } else {
        this.pattern = this.generateECGPattern(4000)
        this.regenTimer = setInterval(() => {
          this.pattern = this.generateECGPattern(4000)
        }, 60 * 1000)
      }
      this.lastTime = performance.now()
      this.start()
    })
    window.addEventListener('resize', this.onResize)
  },
  beforeUnmount() {
    this.stop()
    clearInterval(this.regenTimer)
    window.removeEventListener('resize', this.onResize)
  },
  methods: {
    getBgColor() {
      const colors = {
        dark: 'linear-gradient(180deg, rgba(10,18,35,0.12), rgba(5,10,20,0.06))',
        black: 'linear-gradient(180deg, rgba(0,0,0,0.12), rgba(0,0,0,0.06))',
        white: 'linear-gradient(180deg, rgba(255,255,255,0.12), rgba(255,255,255,0.06))',
        green: 'linear-gradient(180deg, rgba(0,30,0,0.12), rgba(0,20,0,0.06))'
      }
      return colors[this.ecgSettings.bgColor] || colors.dark
    },
    getBackgroundColor() {
      const colors = {
        dark: '#0a0f1e',
        black: '#000000',
        white: '#ffffff',
        green: '#001400'
      }
      return colors[this.ecgSettings.bgColor] || colors.dark
    },
    getWaveColor() {
      const colors = {
        lightblue: { main: '#87CEFA', glow: 'rgba(135,206,250,0.6)', bright: '#b3e0ff' },
        green: { main: '#2ecc71', glow: 'rgba(46,204,113,0.6)', bright: '#5ee0b0' },
        red: { main: '#e74c3c', glow: 'rgba(231,76,60,0.6)', bright: '#ff6b6b' },
        white: { main: '#ffffff', glow: 'rgba(255,255,255,0.6)', bright: '#f0f0f0' }
      }
      return colors[this.ecgSettings.waveColor] || colors.lightblue
    },
    getGridColors() {
      if (this.ecgSettings.bgColor === 'white') {
        return { fine: 'rgba(200,70,70,0.45)', major: 'rgba(180,50,50,0.7)' }
      } else if (this.ecgSettings.bgColor === 'green') {
        return { fine: 'rgba(0,100,0,0.35)', major: 'rgba(0,120,0,0.5)' }
      }
      return { fine: 'rgba(120,50,80,0.35)', major: 'rgba(140,60,90,0.5)' }
    },
    setupCanvas() {
      const canvas = this.$refs.canvas
      this.dpr = window.devicePixelRatio || 1
      this.ctx = canvas.getContext('2d')
      this.onResize()
    },
    onResize() {
      // debounce resize handling
      clearTimeout(this.resizeTimer)
      this.resizeTimer = setTimeout(() => this.doResize(), 120)
    },
    doResize() {
      const canvas = this.$refs.canvas
      const rect = canvas.getBoundingClientRect()
      this.width = Math.max(300, rect.width)
      this.height = Math.max(160, rect.height || 360)
      canvas.width = Math.floor(this.width * this.dpr)
      canvas.height = Math.floor(this.height * this.dpr)
      canvas.style.width = this.width + 'px'
      canvas.style.height = this.height + 'px'
      this.ctx.setTransform(this.dpr, 0, 0, this.dpr, 0, 0)
      // recreate offscreen grid canvas and draw grid once
      this.createGridCanvas()
    },
    createGridCanvas() {
      // create offscreen canvas sized in device pixels
      const g = document.createElement('canvas')
      g.width = Math.floor(this.width * this.dpr)
      g.height = Math.floor(this.height * this.dpr)
      const gctx = g.getContext('2d')
      // set same transform so we can draw using CSS pixel coordinates
      gctx.setTransform(this.dpr, 0, 0, this.dpr, 0, 0)
      // draw grid into offscreen
      this.drawGridTo(gctx)
      this.gridCanvas = g
    },
    start() {
      const loop = (t) => {
        const dt = t - this.lastTime
        this.lastTime = t
        // scroll speed in pixels per second
        const speed = this.speed
        // compute horizontal scale (samples per pixel)
        const len = this.pattern ? this.pattern.length : 1
        const scaleX = this.width > 0 ? len / this.width : 1
        this.offset = (this.offset + (speed * dt) / 1000 * scaleX) % len
        this.draw()
        this.animationId = requestAnimationFrame(loop)
      }
      this.animationId = requestAnimationFrame(loop)
    },
    stop() {
      if (this.animationId) cancelAnimationFrame(this.animationId)
      this.animationId = null
    },
    async loadCSV() {
      try {
        const res = await fetch('/ecg-data.csv')
        if (!res.ok) return false
        const text = await res.text()
        const lines = text.split(/\r?\n/).slice(1).filter(Boolean)
        this.csvValues = lines.map(l => {
          const parts = l.split(',')
          return parseFloat(parts[1]) || 0
        })
        return this.csvValues.length > 0
      } catch (e) {
        return false
      }
    },
    createPatternFromCSV(length) {
      if (!this.csvValues || this.csvValues.length === 0) return this.generateECGPattern(length)
      const csv = this.csvValues
      const csvLen = csv.length
      // compute min/max to normalize
      let min = Infinity, max = -Infinity
      for (let v of csv) { if (v < min) min = v; if (v > max) max = v }
      const range = (max - min) || 1
      const out = new Float32Array(length)
      // tile csv to fill requested length
      for (let i = 0; i < length; i++) {
        const v = csv[i % csvLen]
        out[i] = (v - (min + max) / 2) / range
      }
      // apply light circular smoothing to avoid seam between end and start
      const smoothN = 5
      const half = Math.floor(smoothN / 2)
      const tmp = new Float32Array(length)
      for (let i = 0; i < length; i++) {
        let s = 0
        for (let k = -half; k <= half; k++) {
          const idx = (i + k + length) % length
          s += out[idx]
        }
        tmp[i] = s / smoothN
      }
      return tmp
    },
    drawGridTo(ctx) {
      const w = this.width
      const h = this.height
      
      // 清除画布
      ctx.clearRect(0, 0, w, h)
      
      // 填充背景色
      const bgColor = this.getBackgroundColor()
      ctx.fillStyle = bgColor
      ctx.fillRect(0, 0, w, h)
      
      // 获取网格颜色
      const gridColors = this.getGridColors()
      const fine = 8
      const majorEvery = 5
      
      // 绘制细网格线
      ctx.strokeStyle = gridColors.fine
      ctx.lineWidth = 0.5
      for (let x = 0; x < w; x += fine) {
        ctx.beginPath()
        ctx.moveTo(x + 0.5, 0)
        ctx.lineTo(x + 0.5, h)
        ctx.stroke()
      }
      for (let y = 0; y < h; y += fine) {
        ctx.beginPath()
        ctx.moveTo(0, y + 0.5)
        ctx.lineTo(w, y + 0.5)
        ctx.stroke()
      }
      
      // 绘制粗网格线
      ctx.strokeStyle = gridColors.major
      ctx.lineWidth = 0.8
      const major = fine * majorEvery
      for (let x = 0; x < w; x += major) {
        ctx.beginPath()
        ctx.moveTo(x + 0.5, 0)
        ctx.lineTo(x + 0.5, h)
        ctx.stroke()
      }
      for (let y = 0; y < h; y += major) {
        ctx.beginPath()
        ctx.moveTo(0, y + 0.5)
        ctx.lineTo(w, y + 0.5)
        ctx.stroke()
      }
    },
    draw() {
      const ctx = this.ctx
      const w = this.width
      const h = this.height
      
      // 清除整个画布（使用设备像素坐标）
      ctx.save()
      ctx.setTransform(1, 0, 0, 1, 0, 0) // 重置变换矩阵到单位矩阵
      ctx.clearRect(0, 0, w * this.dpr, h * this.dpr)
      ctx.restore()
      
      // 绘制网格背景
      if (this.gridCanvas) {
        ctx.save()
        ctx.setTransform(1, 0, 0, 1, 0, 0)
        ctx.drawImage(this.gridCanvas, 0, 0, w * this.dpr, h * this.dpr, 0, 0, w * this.dpr, h * this.dpr)
        ctx.restore()
      }
      
      ctx.save()
      // 应用设备像素比变换
      ctx.setTransform(this.dpr, 0, 0, this.dpr, 0, 0)
      
      // 垂直居中
      ctx.translate(0, h / 2)
      const scaleY = h * 0.14
      const len = this.pattern.length
      const scaleX = w > 0 ? len / w : 1
      
      const waveColor = this.getWaveColor()
      
      // 绘制主波形（带发光效果）
      ctx.beginPath()
      for (let x = 0; x < w; x++) {
        const idx = Math.floor((this.offset + x * scaleX) % len)
        const v = this.pattern[idx]
        const y = -v * scaleY
        if (x === 0) ctx.moveTo(x, y)
        else ctx.lineTo(x, y)
      }
      
      ctx.shadowColor = waveColor.glow
      ctx.shadowBlur = 4
      ctx.lineWidth = 2.5
      ctx.strokeStyle = waveColor.main
      ctx.lineJoin = 'round'
      ctx.lineCap = 'round'
      ctx.stroke()
      
      // 绘制高亮波形（无发光）
      ctx.shadowBlur = 0
      ctx.beginPath()
      for (let x = 0; x < w; x++) {
        const idx = Math.floor((this.offset + x * scaleX) % len)
        const v = this.pattern[idx]
        const y = -v * scaleY
        if (x === 0) ctx.moveTo(x, y)
        else ctx.lineTo(x, y)
      }
      ctx.lineWidth = 1.2
      ctx.strokeStyle = waveColor.bright
      ctx.stroke()
      
      ctx.restore()
      
      // 更新显示的数值
      if (this.ecgSettings.showData) {
        const idx = Math.floor(this.offset % len)
        this.currentVoltage = (this.pattern[idx] * 1.2).toFixed(2)
      }
    },
    updateDisplay() {
      if (this.gridCanvas) {
        this.createGridCanvas()
      }
    },
    exportData() {
      if (this.csvValues && this.csvValues.length > 0) {
        const csvContent = '时间(秒),II导联电压(mV)\n' + 
          this.csvValues.map((v, i) => `${(i * 0.008).toFixed(3)},${v}`).join('\n')
        const blob = new Blob([csvContent], { type: 'text/csv' })
        const url = window.URL.createObjectURL(blob)
        const a = document.createElement('a')
        a.href = url
        a.download = 'ecg-data.csv'
        a.click()
        window.URL.revokeObjectURL(url)
      }
    },
    // generate synthetic ECG-like pattern: create a single beat then tile to length
    generateECGPattern(length) {
      const out = new Float32Array(length)
      // target heart rate
      const hr = 65 + Math.floor(Math.random() * 10)
      this.heartRate = hr
      const beatsPerMinute = hr
      const secondsPerBeat = 60 / beatsPerMinute
      // assume sample density ~ 250 samples per second
      const sampleRate = 250
      const beatSamples = Math.max(160, Math.floor(sampleRate * secondsPerBeat))
      // build single beat waveform
      const beat = new Float32Array(beatSamples)
      for (let i = 0; i < beatSamples; i++) {
        const t = i / beatSamples
        let v = 0
        // small P wave around 0.12
        v += Math.exp(-Math.pow((t - 0.12) * 40, 2)) * 0.06
        // small Q dip before main spike
        v -= Math.exp(-Math.pow((t - 0.32) * 80, 2)) * 0.06
        // R spike (sharp)
        v += Math.exp(-Math.pow((t - 0.35) * 200, 2)) * (0.95 + Math.random() * 0.08)
        // S small negative after R
        v -= Math.exp(-Math.pow((t - 0.37) * 120, 2)) * 0.08
        // T wave broad
        v += Math.exp(-Math.pow((t - 0.55) * 25, 2)) * 0.12
        // baseline noise
        v += (Math.random() - 0.5) * 0.02
        beat[i] = v
      }
      // tile beat to fill requested length, add short gaps to mimic rhythm
      let pos = 0
      while (pos < length) {
        for (let i = 0; i < beatSamples && pos < length; i++, pos++) out[pos] = beat[i]
        // small post-beat decay / flatline for spacing (10-30 samples)
        const gap = Math.floor(sampleRate * 0.02 + Math.random() * (sampleRate * 0.03))
        for (let g = 0; g < gap && pos < length; g++, pos++) out[pos] = (Math.random() - 0.5) * 0.01
      }
      return out
    }
  }
}
</script>

<style scoped>
.ecg-card { padding:18px; display:flex; flex-direction:column; gap:12px; min-height:420px; }
.ecg-title { font-size:1.2rem; color:#dfe8ff; font-weight:600; }
.ecg-plot { flex:1; background: linear-gradient(180deg, rgba(10,18,35,0.12), rgba(5,10,20,0.06)); border-radius:12px; padding:12px; display:flex; align-items:center; position: relative; }
.ecg-canvas { width:100%; height:100%; border-radius:8px; display:block }
.ecg-data-display {
  position: absolute;
  top: 20px;
  right: 20px;
  background: rgba(0, 0, 0, 0.7);
  padding: 12px 16px;
  border-radius: 8px;
  color: #4ecdc4;
  font-size: 0.9rem;
  font-family: monospace;
}
.data-item {
  margin: 4px 0;
}
</style>