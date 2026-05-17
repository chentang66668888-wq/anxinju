<template>
  <div class="dashboard">
    <div class="logout-bar">
      <span class="user-info">👤 {{ currentUserData }}</span>
      <button class="btn-logout" @click="handleLogout">退出登录</button>
    </div>
    
    <div class="app-layout">
      <Sidebar @select="onSidebarSelect" :selected="selectedModule" />

      <div class="main-area">
        <div class="module-panel" v-if="selectedModule && selectedModule !== 'info'">
          <div class="module-wrapper glass-card">
            <SmartDevices v-if="selectedModule === 'devices'" />
            <BrandOverview v-if="selectedModule === 'brand'" />
            <div v-if="selectedModule === 'settings'" class="settings-full">
              <Settings />
            </div>
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
import Settings from './Settings.vue'

export default {
  name: 'AnxDashboard',
  components: { Sidebar, TopStats, UserCard, StatusBar, AnxECG, AnxRouteMap, SmartDevices, BrandOverview, Settings },
  props: {
    currentUser: {
      type: String,
      default: ''
    }
  },
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
      },
      currentUserData: this.currentUser || localStorage.getItem('anxju_current_user') || '用户'
    }
  },
  methods: {
    onSidebarSelect(key) {
      this.selectedModule = this.selectedModule === key ? null : key
    },
    handleUserInfoUpdate(userInfo) {
      this.userInfo = { ...userInfo }
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
    },
    handleLogout() {
      localStorage.removeItem('anxju_logged_in')
      localStorage.removeItem('anxju_current_user')
      this.$emit('logout')
    }
  }
}
</script>

<style scoped>
@import '../styles/dashboard-beautified.css';

.dashboard { 
    width: 100%;
    max-width: 1800px; 
    margin: 0 auto; 
    padding: 24px 32px;
    display: flex; 
    flex-direction: column; 
    gap: 24px;
    flex: 1;
    overflow-y: auto;
}

.logout-bar { 
    display: flex; 
    justify-content: space-between; 
    align-items: center; 
    padding: 14px 28px; 
    background: rgba(18, 25, 45, 0.7); 
    backdrop-filter: blur(12px); 
    border-radius: 18px; 
    border: 1px solid rgba(72, 120, 255, 0.25);
    box-shadow: 0 4px 20px rgba(0, 0, 0, 0.2);
    flex-shrink: 0;
}

.user-info { 
    font-size: 0.95rem; 
    color: #eef2ff; 
    font-weight: 600;
    display: flex;
    align-items: center;
    gap: 8px;
}

.btn-logout { 
    background: rgba(255, 123, 114, 0.15); 
    border: 1px solid rgba(255, 123, 114, 0.4); 
    color: #ff9f8f; 
    padding: 10px 24px; 
    border-radius: 24px; 
    cursor: pointer; 
    font-size: 0.9rem; 
    font-weight: 600;
    transition: all 0.3s ease;
    flex-shrink: 0;
}

.btn-logout:hover { 
    background: rgba(255, 123, 114, 0.3); 
    border-color: #ff7b72; 
    color: #fff;
    transform: translateY(-2px);
    box-shadow: 0 6px 20px rgba(255, 123, 114, 0.3);
}

.app-layout { 
    display: grid; 
    grid-template-columns: 220px 1fr; 
    gap: 28px; 
    align-items: start;
    flex: 1;
    min-height: 0;
}

.main-area { 
    display: flex; 
    flex-direction: column; 
    gap: 28px; 
    min-width: 0;
    flex: 1;
    min-height: 0;
}

.stats-row {
    flex-shrink: 0;
}

.middle-section { 
    display: grid; 
    grid-template-columns: 1fr 400px; 
    gap: 28px;
    align-items: start;
    flex-shrink: 0;
}

.center-ecg { 
    display: flex; 
    align-items: flex-start; 
    gap: 28px;
    flex: 1;
}

.center-wrapper { 
    flex: 1; 
    min-width: 0; 
    display: flex;
}

.center-wrapper > * { 
    width: 100%;
}

.center-ecg .user-card { 
    width: 400px; 
    margin-left: auto;
    flex-shrink: 0;
}

.route-map-container {
    flex-shrink: 0;
}

.status-bar-container {
    flex-shrink: 0;
    margin-top: auto;
}

.module-panel {
    animation: fadeIn 0.4s ease;
    flex: 1;
    display: flex;
    flex-direction: column;
}

@keyframes fadeIn {
    from {
        opacity: 0;
        transform: translateY(10px);
    }
    to {
        opacity: 1;
        transform: translateY(0);
    }
}

.module-wrapper { 
    padding: 20px; 
    border-radius: 20px; 
    background: rgba(18, 25, 45, 0.7); 
    border: 1px solid rgba(72, 120, 255, 0.1);
    backdrop-filter: blur(15px);
    margin-bottom: 8px;
    box-shadow: 0 8px 25px rgba(0, 0, 0, 0.2);
    flex: 1;
    display: flex;
    flex-direction: column;
}

/* ====== 大屏优化 (>1400px) ====== */
@media (min-width: 1401px) {
    .dashboard {
        padding: 28px 40px;
    }
}

/* ====== 中屏适配 (1200-1400px) ====== */
@media (max-width: 1400px) {
    .dashboard {
        padding: 20px 28px;
    }
    
    .app-layout {
        grid-template-columns: 200px 1fr;
        gap: 24px;
    }
    
    .middle-section {
        grid-template-columns: 1fr;
        gap: 24px;
    }
    
    .center-ecg .user-card {
        width: 100%;
        margin-left: 0;
    }
}

/* ====== 小屏适配 (<1200px) ====== */
@media (max-width: 1200px) {
    .dashboard {
        padding: 18px 24px;
        gap: 20px;
    }
    
    .app-layout {
        grid-template-columns: 1fr;
        gap: 20px;
    }
    
    .main-area {
        gap: 20px;
    }
    
    .middle-section { 
        gap: 20px; 
    }
    
    .center-ecg { 
        flex-direction: column;
        gap: 20px;
    }
    
    .logout-bar {
        padding: 12px 20px;
    }
}

/* ====== 移动端适配 (<768px) ====== */
@media (max-width: 768px) {
    .dashboard {
        padding: 12px 16px;
        gap: 16px;
    }
    
    .logout-bar {
        padding: 10px 16px;
        flex-wrap: wrap;
        gap: 12px;
    }
    
    .user-info {
        font-size: 0.85rem;
    }
    
    .btn-logout {
        padding: 8px 18px;
        font-size: 0.85rem;
    }
    
    .stats-row {
        gap: 12px;
    }
    
    .middle-section {
        gap: 16px;
    }
    
    .center-ecg {
        gap: 16px;
    }
}

/* ====== 超小屏适配 (<480px) ====== */
@media (max-width: 480px) {
    .dashboard {
        padding: 8px 12px;
        gap: 12px;
    }
    
    .logout-bar {
        padding: 8px 12px;
    }
    
    .stats-row {
        gap: 10px;
    }
}
</style>