<template>
  <div class="route-card glass-card">
    <div class="route-title">路线图</div>
    <div class="route-plot">
      <canvas ref="canvas" class="route-canvas"></canvas>
    </div>
  </div>
</template>

<script>
export default {
  name: 'AnxRouteMap',
  data() {
    return {
      ctx: null,
      width: 0,
      height: 0,
      dpr: 1,
      routeData: null,
      resizeTimer: null
    }
  },
  mounted() {
    this.setupCanvas()
    this.generateRouteData()
    this.draw()
    window.addEventListener('resize', this.onResize)
  },
  beforeUnmount() {
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
      clearTimeout(this.resizeTimer)
      this.resizeTimer = setTimeout(() => this.doResize(), 120)
    },
    doResize() {
      const canvas = this.$refs.canvas
      const rect = canvas.getBoundingClientRect()
      this.width = Math.max(300, rect.width)
      this.height = Math.max(200, rect.height || 280)
      canvas.width = Math.floor(this.width * this.dpr)
      canvas.height = Math.floor(this.height * this.dpr)
      canvas.style.width = this.width + 'px'
      canvas.style.height = this.height + 'px'
      this.ctx.setTransform(this.dpr, 0, 0, this.dpr, 0, 0)
      this.draw()
    },
    generateRouteData() {
      // 生成模拟路线图数据
      // 路线点坐标（相对于画布大小的比例）
      this.routeData = {
        // 主要路线点（模拟GPS轨迹）
        routePoints: [
          {x: 0.15, y: 0.60}, {x: 0.20, y: 0.58}, {x: 0.25, y: 0.57},
          {x: 0.30, y: 0.56}, {x: 0.35, y: 0.55}, {x: 0.40, y: 0.54},
          {x: 0.45, y: 0.53}, {x: 0.50, y: 0.52}, {x: 0.55, y: 0.51},
          {x: 0.60, y: 0.50}, {x: 0.65, y: 0.49}, {x: 0.70, y: 0.48},
          {x: 0.75, y: 0.47}, {x: 0.78, y: 0.45}, {x: 0.80, y: 0.42},
          {x: 0.82, y: 0.40}, {x: 0.84, y: 0.38}, {x: 0.86, y: 0.36},
          {x: 0.88, y: 0.35}, {x: 0.90, y: 0.34}
        ],
        // 终点位置
        endMarker: {x: 0.90, y: 0.34},
        // 河流路径
        rivers: [
          [{x: 0.30, y: 0.0}, {x: 0.35, y: 0.15}, {x: 0.38, y: 0.30}, {x: 0.36, y: 0.50}, {x: 0.32, y: 0.70}, {x: 0.30, y: 1.0}],
          [{x: 0.60, y: 0.0}, {x: 0.58, y: 0.20}, {x: 0.55, y: 0.40}, {x: 0.58, y: 0.60}, {x: 0.62, y: 0.80}, {x: 0.65, y: 1.0}]
        ],
        // 道路网格
        roads: {
          horizontal: [0.25, 0.45, 0.65, 0.80],
          vertical: [0.20, 0.40, 0.60, 0.80]
        },
        // 区域标签
        labels: [
          {text: '软件园', x: 0.35, y: 0.15},
          {text: '高新区', x: 0.70, y: 0.60},
          {text: '科技园', x: 0.50, y: 0.85}
        ]
      }
    },
    draw() {
      if (!this.ctx || !this.routeData) return
      const ctx = this.ctx
      const w = this.width
      const h = this.height
      
      // 清空画布
      ctx.clearRect(0, 0, w, h)
      
      // 绘制浅色背景
      ctx.fillStyle = '#f8f6f0'
      ctx.fillRect(0, 0, w, h)
      
      // 绘制河流
      this.drawRivers(ctx, w, h)
      
      // 绘制道路网格
      this.drawRoads(ctx, w, h)
      
      // 绘制路线
      this.drawRoute(ctx, w, h)
      
      // 绘制终点标记
      this.drawEndMarker(ctx, w, h)
      
      // 绘制标签
      this.drawLabels(ctx, w, h)
    },
    drawRivers(ctx, w, h) {
      ctx.strokeStyle = 'rgba(180, 210, 240, 0.6)'
      ctx.lineWidth = 8
      ctx.lineCap = 'round'
      ctx.lineJoin = 'round'
      
      this.routeData.rivers.forEach(river => {
        ctx.beginPath()
        river.forEach((point, index) => {
          const x = point.x * w
          const y = point.y * h
          if (index === 0) ctx.moveTo(x, y)
          else ctx.lineTo(x, y)
        })
        ctx.stroke()
      })
    },
    drawRoads(ctx, w, h) {
      ctx.strokeStyle = 'rgba(230, 225, 200, 0.8)'
      ctx.lineWidth = 6
      ctx.lineCap = 'round'
      
      // 横向道路
      this.routeData.roads.horizontal.forEach(y => {
        ctx.beginPath()
        ctx.moveTo(0, y * h)
        ctx.lineTo(w, y * h)
        ctx.stroke()
      })
      
      // 纵向道路
      this.routeData.roads.vertical.forEach(x => {
        ctx.beginPath()
        ctx.moveTo(x * w, 0)
        ctx.lineTo(x * w, h)
        ctx.stroke()
      })
    },
    drawRoute(ctx, w, h) {
      // 绘制路线（青绿色）
      ctx.strokeStyle = '#4ecdc4'
      ctx.lineWidth = 10
      ctx.lineCap = 'round'
      ctx.lineJoin = 'round'
      ctx.shadowColor = 'rgba(78, 205, 196, 0.4)'
      ctx.shadowBlur = 8
      
      ctx.beginPath()
      this.routeData.routePoints.forEach((point, index) => {
        const x = point.x * w
        const y = point.y * h
        if (index === 0) ctx.moveTo(x, y)
        else ctx.lineTo(x, y)
      })
      ctx.stroke()
      
      // 重置shadow
      ctx.shadowBlur = 0
    },
    drawEndMarker(ctx, w, h) {
      const marker = this.routeData.endMarker
      const x = marker.x * w
      const y = marker.y * h
      
      // 绘制红色位置标记
      ctx.fillStyle = '#e74c3c'
      ctx.beginPath()
      ctx.arc(x, y - 8, 12, 0, Math.PI * 2)
      ctx.fill()
      
      // 白色内圈
      ctx.fillStyle = '#ffffff'
      ctx.beginPath()
      ctx.arc(x, y - 8, 5, 0, Math.PI * 2)
      ctx.fill()
    },
    drawLabels(ctx, w, h) {
      ctx.fillStyle = 'rgba(100, 100, 100, 0.7)'
      ctx.font = '12px sans-serif'
      ctx.textAlign = 'center'
      ctx.textBaseline = 'middle'
      
      this.routeData.labels.forEach(label => {
        const x = label.x * w
        const y = label.y * h
        ctx.fillText(label.text, x, y)
      })
    }
  }
}
</script>

<style scoped>
.route-card { padding:18px; display:flex; flex-direction:column; gap:12px; min-height:320px; }
.route-title { font-size:1.1rem; color:#dfe8ff; font-weight:600; text-align:center }
.route-plot { flex:1; background: rgba(255,255,255,0.05); border-radius:12px; display:flex; align-items:center; justify-content:center; }
.route-canvas { width:100%; height:100%; border-radius:8px; display:block }
</style>
