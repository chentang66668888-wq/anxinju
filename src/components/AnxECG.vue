<template>
  <div class="ecg-card glass-card">
    <div class="ecg-title">心电图</div>
    <div class="ecg-plot">
      <canvas ref="canvas" class="ecg-canvas"></canvas>
    </div>
  </div>
</template>

<script>
export default {
  name: 'AnxECG',
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
      regenTimer: null
    }
  },
  mounted() {
    this.setupCanvas()
    // try load CSV data from public folder; fallback to synthetic
    this.loadCSV().then(ok => {
      if (ok) {
        this.pattern = this.createPatternFromCSV(4000)
      } else {
        this.pattern = this.generateECGPattern(4000)
        // regenerate synthetic pattern every minute
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
        const speed = 40
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
      ctx.clearRect(0, 0, w, h)
      // paper background (white)
      ctx.fillStyle = '#fff'
      ctx.fillRect(0, 0, w, h)
      // grid configuration: fine and major (every 5 fine lines)
      const fine = 8
      const majorEvery = 5
      // fine grid (red)
      ctx.strokeStyle = 'rgba(200,70,70,0.45)'
      ctx.lineWidth = 1
      for (let x = 0; x < w; x += fine) {
        ctx.beginPath(); ctx.moveTo(x + 0.5, 0); ctx.lineTo(x + 0.5, h); ctx.stroke()
      }
      for (let y = 0; y < h; y += fine) {
        ctx.beginPath(); ctx.moveTo(0, y + 0.5); ctx.lineTo(w, y + 0.5); ctx.stroke()
      }
      // major grid lines (darker red)
      ctx.strokeStyle = 'rgba(180,50,50,0.7)'
      ctx.lineWidth = 1.6
      const major = fine * majorEvery
      for (let x = 0; x < w; x += major) {
        ctx.beginPath(); ctx.moveTo(x + 0.5, 0); ctx.lineTo(x + 0.5, h); ctx.stroke()
      }
      for (let y = 0; y < h; y += major) {
        ctx.beginPath(); ctx.moveTo(0, y + 0.5); ctx.lineTo(w, y + 0.5); ctx.stroke()
      }
    },
    draw() {
      const ctx = this.ctx
      const w = this.width
      const h = this.height
      // draw cached grid first (faster than redrawing)
      if (this.gridCanvas) ctx.drawImage(this.gridCanvas, 0, 0, this.gridCanvas.width, this.gridCanvas.height, 0, 0, w * this.dpr, h * this.dpr)
      // draw waveform centered vertically
      ctx.save()
      ctx.translate(0, h / 2)
      ctx.beginPath()
      const scaleY = h * 0.22
      const len = this.pattern.length
      // horizontal scaling: samples per pixel so pattern fills width proportionally
      const scaleX = w > 0 ? len / w : 1
      for (let x = 0; x < w; x++) {
        const idx = Math.floor((this.offset + x * scaleX) % len)
        const v = this.pattern[idx]
        const y = -v * scaleY
        if (x === 0) ctx.moveTo(x, y)
        else ctx.lineTo(x, y)
      }
      // draw filled area under curve to baseline
      ctx.lineJoin = 'round'
      ctx.lineCap = 'round'
      ctx.beginPath()
      for (let x = 0; x < w; x++) {
        const idx = Math.floor((this.offset + x) % len)
        const v = this.pattern[idx]
        const y = -v * scaleY
        if (x === 0) ctx.moveTo(x, y)
        else ctx.lineTo(x, y)
      }
      // close path to baseline to create fill
      ctx.lineTo(w, 0)
      ctx.lineTo(0, 0)
      ctx.closePath()
      ctx.fillStyle = 'rgba(0,0,0,0.04)'
      ctx.fill()

      // draw thick black stroke and a thinner light center to mimic printed ECG line
      ctx.beginPath()
      for (let x = 0; x < w; x++) {
        const idx = Math.floor((this.offset + x) % len)
        const v = this.pattern[idx]
        const y = -v * scaleY
        if (x === 0) ctx.moveTo(x, y)
        else ctx.lineTo(x, y)
      }
      ctx.lineWidth = 6.0
      ctx.strokeStyle = '#000'
      ctx.shadowColor = 'rgba(0,0,0,0.08)'
      ctx.shadowBlur = 2
      ctx.stroke()
      // overlay a slightly thinner light stroke for the inner highlight
      ctx.beginPath()
      for (let x = 0; x < w; x++) {
        const idx = Math.floor((this.offset + x) % len)
        const v = this.pattern[idx]
        const y = -v * scaleY
        if (x === 0) ctx.moveTo(x, y)
        else ctx.lineTo(x, y)
      }
      ctx.lineWidth = 2.0
      ctx.shadowBlur = 0
      ctx.strokeStyle = '#dcdcdc'
      ctx.stroke()
      ctx.restore()
    },
    // generate synthetic ECG-like pattern: create a single beat then tile to length
    generateECGPattern(length) {
      const out = new Float32Array(length)
      // target heart rate
      const hr = 65 + Math.floor(Math.random() * 10) // 65-74 bpm
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
.ecg-plot { flex:1; background: linear-gradient(180deg, rgba(10,18,35,0.12), rgba(5,10,20,0.06)); border-radius:12px; padding:12px; display:flex; align-items:center; }
.ecg-canvas { width:100%; height:100%; border-radius:8px; display:block }
</style>
