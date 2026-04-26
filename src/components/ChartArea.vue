<template>
  <div class="chart-area glass-card">
    <div id="trendChart" class="chart-box"></div>
    <div class="risk-box">
      <div class="risk-title">
        <span>🧬 健康与行为风险图谱</span>
        <span style="font-size:11px;">(雷达图 | 实时维度)</span>
      </div>
      <div id="radarChart" style="height:200px; width:100%;"></div>
    </div>
    <div style="font-size: 0.7rem; text-align: right; color:#5a73a3;">基于毫米波雷达与生物传感器动态建模</div>
  </div>
</template>

<script>
export default {
  name: 'ChartArea',
  data() { return { trendChart: null, radarChart: null } },
  methods: {
    loadEcharts() {
      return new Promise((resolve) => {
        if (window.echarts) return resolve(window.echarts)
        const s = document.createElement('script')
        s.src = 'https://cdn.jsdelivr.net/npm/echarts@5.5.0/dist/echarts.min.js'
        s.onload = () => resolve(window.echarts)
        document.head.appendChild(s)
      })
    },
    initCharts() {
      if (!window.echarts) return
      this.trendChart = window.echarts.init(document.getElementById('trendChart'))
      this.radarChart = window.echarts.init(document.getElementById('radarChart'))

      this.trendChart.setOption({
        tooltip: { trigger: 'axis', axisPointer: { type: 'shadow' }, backgroundColor: 'rgba(10,20,40,0.9)', borderColor: '#3b6eff' },
        grid: { top: 25, left: 45, right: 15, bottom: 20, containLabel: true },
        xAxis: { type: 'category', data: ['2/10','2/11','2/12','2/13','2/14','2/15','2/16','2/17'], axisLabel:{ color:'#9aa9c7' }, axisLine:{ lineStyle:{ color:'#2a3a60'}} },
        yAxis: { type: 'value', name: '安全评分', nameTextStyle:{ color:'#b9c8ff' }, splitLine:{ lineStyle:{ color:'#1f2a44', type:'dashed' } }, axisLabel:{ color:'#cbd5e6' } },
        series: [{ data:[78,82,81,79,74,68,72,86], type:'line', smooth:true, symbol:'circle', symbolSize:8, lineStyle:{ width:3, color:'#3b6eff', shadowBlur:12 }, areaStyle:{ opacity:0.3, color: new window.echarts.graphic.LinearGradient(0,0,0,1,[{offset:0,color:'#3b6eff'},{offset:1,color:'#14223e'}]) }, markPoint:{ data:[{ type:'min', name:'摔倒预警' }], symbolSize:40, label:{ formatter:'⚠️ 低分', color:'#ffaa66' } }, itemStyle:{ color:'#5d9eff' } }],
        backgroundColor: 'transparent'
      })

      this.radarChart.setOption({
        radar: { indicator:[{ name:'摔倒风险', max:100 },{ name:'夜间如厕频率', max:100 },{ name:'心电图异常趋势', max:100 },{ name:'空气质量影响指数', max:100 },{ name:'路径偏离日常', max:100 }], shape:'circle', center:['50%','50%'], radius:'68%', name:{ textStyle:{ color:'#b9d0ff', fontSize:10 } }, splitArea:{ areaStyle:{ color:['rgba(59,110,255,0.1)','rgba(35,75,155,0.2)'] } }, axisLine:{ lineStyle:{ color:'#2a4270' } } },
        series: [{ type:'radar', data:[{ value:[68,72,55,45,63], name:'当前长者风险' }], areaStyle:{ color:'rgba(59,110,255,0.3)' }, lineStyle:{ width:2, color:'#6d9eff' }, itemStyle:{ color:'#ffb347' } }],
        tooltip: { trigger: 'item' }, backgroundColor: 'transparent'
      })
    },
    onResize() { if (this.trendChart) this.trendChart.resize(); if (this.radarChart) this.radarChart.resize(); }
  },
  mounted() {
    this.loadEcharts().then(() => {
      this.initCharts()
      window.addEventListener('resize', this.onResize)
      // 模拟 5s 后数据更新
      setTimeout(() => {
        if (this.trendChart) this.trendChart.setOption({ series:[{ data:[80,83,85,82,79,73,78,88] }] })
        if (this.radarChart) this.radarChart.setOption({ series:[{ data:[{ value:[65,74,52,48,68] }] }] })
      }, 5000)
    })
  },
  beforeUnmount() {
    window.removeEventListener('resize', this.onResize)
    if (this.trendChart) this.trendChart.dispose()
    if (this.radarChart) this.radarChart.dispose()
  },
  
}
</script>

<style scoped>
/* container sizing already handled by parent */
</style>
