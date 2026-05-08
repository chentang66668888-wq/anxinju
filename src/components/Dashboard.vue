<template>
  <div class="dashboard app-layout">
    <Sidebar @select="onSidebarSelect" :selected="selectedModule" />

    <div class="main-area">
      <div class="module-panel" v-if="selectedModule && selectedModule !== 'info'">
        <div class="module-wrapper glass-card">
          <SmartDevices v-if="selectedModule === 'devices'" />
          <BrandOverview v-if="selectedModule === 'brand'" />
          <div v-if="selectedModule === 'settings'" class="settings-full">设置面板（占位）</div>
        </div>
      </div>

      <!-- 信息中心使用默认主布局（TopStats + 心率图 + 用户卡），因此不渲染单独的 InfoCenter 面板 -->

      <div v-if="!selectedModule || selectedModule === 'info'">
        <div class="stats-row">
          <TopStats />
        </div>

        <div class="middle-section center-ecg">
          <div class="center-wrapper">
            <AnxECG ref="ecgComponent" :ecg-settings="ecgSettings" />
          </div>
          <UserCard 
            @update-user-info="handleUserInfoUpdate"
            @update-ecg-settings="handleECGSettingsUpdate"
            @update-route-settings="handleRouteSettingsUpdate"
            @export-ecg-data="handleExportECGData"
          />
        </div>

        <AnxRouteMap :route-settings="routeSettings" />

        <StatusBar />
      </div>
    </div>
  </div>
</template>

<script>
import Sidebar from './Sidebar.vue'
import TopStats from './TopStats.vue'
import UserCard from './UserCard.vue'
import StatusBar from './StatusBar.vue'
import AnxECG from './AnxECG.vue'
import AnxRouteMap from './AnxRouteMap.vue'
import SmartDevices from './SmartDevices.vue'
import BrandOverview from './BrandOverview.vue'

export default {
  name: 'AnxDashboard',
  components: { Sidebar, TopStats, UserCard, StatusBar, AnxECG, AnxRouteMap, SmartDevices, BrandOverview },
  data() {
    return { 
      selectedModule: 'info',
      ecgSettings: {
        bgColor: 'dark',
        showData: false,
        waveColor: 'lightblue'
      },
      routeSettings: {
        showLabels: true,
        showRivers: true,
        showRoads: true,
        routeColor: 'cyan'
      },
      userInfo: {
        name: '张德海',
        age: 78,
        healthCondition: '高血压、轻度认知障碍、夜间需关注如厕'
      }
    }
  },
  methods: {
    onSidebarSelect(key) {
      this.selectedModule = this.selectedModule === key ? null : key
    },
    handleUserInfoUpdate(userInfo) {
      this.userInfo = { ...userInfo }
      // 可以在这里将用户信息保存到 localStorage 或发送到后端
      console.log('用户信息已更新:', this.userInfo)
    },
    handleECGSettingsUpdate(ecgSettings) {
      this.ecgSettings = { ...ecgSettings }
      console.log('心电图设置已更新:', this.ecgSettings)
    },
    handleRouteSettingsUpdate(routeSettings) {
      this.routeSettings = { ...routeSettings }
      console.log('路线图设置已更新:', this.routeSettings)
    },
    handleExportECGData() {
      if (this.$refs.ecgComponent) {
        this.$refs.ecgComponent.exportData()
      }
      console.log('导出心电图数据')
    }
  }
}
</script>

<style>
/* ====== 全局布局与组件样式（来自示意图） ====== */
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    background: linear-gradient(145deg, #0a0f1e 0%, #0c1222 100%);
    font-family: 'Inter', 'Segoe UI', 'PingFang SC', Roboto, 'Helvetica Neue', sans-serif;
    padding: 24px 32px;
    color: #eef2ff;
}

.glass-card {
    background: rgba(18, 25, 45, 0.65);
    backdrop-filter: blur(12px);
    border-radius: 28px;
    border: 1px solid rgba(72, 120, 255, 0.2);
    box-shadow: 0 15px 35px rgba(0, 0, 0, 0.3);
    transition: all 0.2s ease;
}
.glass-card:hover { border-color: rgba(72, 120, 255, 0.5); box-shadow: 0 20px 40px rgba(0,0,0,0.4); }

.dashboard { max-width: 1600px; margin: 0 auto; display: flex; flex-direction: column; gap: 24px; }
.app-layout { display: grid; grid-template-columns: 120px 1fr; gap: 24px; align-items: start; }
.main-area { display: flex; flex-direction: column; gap: 24px; }
.stats-row { display: grid; grid-template-columns: repeat(4, 1fr); gap: 24px; }
.stat-card { padding: 20px 20px; display:flex; flex-direction:column; justify-content:space-between; }
.stat-title { font-size:0.9rem; font-weight:500; text-transform:uppercase; letter-spacing:1px; color:#9aa9c7; margin-bottom:12px; }
.stat-value { font-size:2.6rem; font-weight:700; color:white; line-height:1.2; }
.stat-trend { font-size:0.75rem; margin-top:10px; color:#6c86a3; display:flex; align-items:center; gap:6px; }
.trend-up { color:#ff7b72; } .trend-down { color:#5ee0b0; }
.badge { background: rgba(72,120,255,0.2); padding:2px 8px; border-radius:20px; font-size:0.7rem; }

  .middle-section { display:grid; grid-template-columns: 320px 1fr 360px; gap:24px; }
  .middle-section.two-col { grid-template-columns: 1fr 420px; align-items:start }
  .ecg-card { min-height:420px }
  .center-ecg { display:flex; align-items:flex-start; gap:24px }
  .center-wrapper { flex:1; min-width:0; display:flex }
  .center-wrapper > * { width:100% }
  /* ensure user card stays fixed width and aligns to right */
  .center-ecg .user-card { width:360px; margin-left:auto }
.route-card { grid-column: 1 / -1 }

/* expanded module panel */
.module-panel { }
.module-wrapper { padding:12px; border-radius:16px; background: rgba(18,25,45,0.6); border:1px solid rgba(72,120,255,0.06); margin-bottom:6px }
.module-wrapper .brand-full { padding:12px; color:#dfe8ff }
.module-wrapper .brand-full-title { font-weight:700; margin-bottom:8px }
.module-wrapper .settings-full { padding:12px; color:#cbd5e6 }
.user-card { padding:24px 20px; display:flex; flex-direction:column; gap:20px; }
.user-avatar { width:56px; height:56px; background:linear-gradient(135deg,#3b6eff,#8b5cf6); border-radius:30px; display:flex; align-items:center; justify-content:center; font-size:28px; margin-bottom:8px; }
.user-name { font-size:1.6rem; font-weight:600; }
.user-detail { border-top:1px solid rgba(255,255,255,0.1); padding-top:16px; font-size:0.85rem; line-height:1.5; color:#cbd5e6; }
.setting-buttons { display:flex; flex-direction:column; gap:12px; margin-top:8px; }
.btn-outline { background: rgba(255,255,255,0.05); border:1px solid rgba(72,120,255,0.4); border-radius:40px; padding:10px 0; text-align:center; font-size:0.85rem; font-weight:500; cursor:pointer; transition:0.2s; color:#cddcff; }
.btn-outline:hover { background: rgba(72,120,255,0.25); border-color: #3b6eff }

.chart-area { padding:20px; display:flex; flex-direction:column; gap:24px; }
.chart-box { width:100%; height:210px; }
.risk-box { margin-top:8px; background: rgba(0,0,0,0.3); border-radius:20px; padding:12px 16px; }
.risk-title { font-size:0.8rem; font-weight:600; margin-bottom:12px; color:#b9c8ff; display:flex; align-items:center; gap:8px; }

.log-panel { padding:20px; display:flex; flex-direction:column; gap:16px; }
.log-header { font-weight:600; font-size:1rem; border-left:3px solid #3b6eff; padding-left:12px; }
.log-list { display:flex; flex-direction:column; gap:14px; max-height:380px; overflow-y:auto; padding-right:4px; }
.log-item { font-size:0.8rem; font-family: monospace; border-bottom:1px solid rgba(255,255,255,0.05); padding-bottom:10px; display:flex; gap:10px; }
.log-time { color:#5b7bc3; min-width:68px; }
.log-text { color:#e2e8ff; }
.log-badge { background: rgba(255,123,114,0.2); color:#ff9f8f; border-radius:14px; padding:0px 8px; font-size:0.7rem; margin-left:8px; }

.status-bar { margin-top:8px; padding:14px 24px; display:flex; flex-wrap:wrap; justify-content:space-between; align-items:center; font-size:0.8rem; background: rgba(8,14,26,0.8); backdrop-filter: blur(8px); border-radius:40px; border:1px solid rgba(72,120,255,0.25); }
.status-left { display:flex; gap:24px; flex-wrap:wrap; }
.status-dot { display:inline-block; width:8px; height:8px; border-radius:10px; margin-right:6px; }
.green-dot { background:#2ecc71; box-shadow:0 0 6px #2ecc71; } .blue-dot { background:#3b6eff; }
.sync-time { color:#7f8fa4; }

::-webkit-scrollbar { width:4px; }
::-webkit-scrollbar-track { background:#1e2a3a; border-radius:4px; }
::-webkit-scrollbar-thumb { background:#3b6eff; border-radius:4px; }

@media (max-width:1200px) {
  body { padding:16px; }
  .middle-section { grid-template-columns:1fr; gap:20px; }
  .stats-row { gap:16px; }
}
</style>