<template>
  <aside class="sidebar glass-card">
    <!-- 顶部功能按钮 -->
    <div class="top-buttons">
      <div 
        @click="$emit('select','info')" 
        :class="['nav-item', {active: selected === 'info'}]"
      >
        <span class="nav-icon">📊</span>
        <span class="nav-label">信息中心</span>
        <div class="active-indicator"></div>
      </div>
      
      <div 
        @click="$emit('select','devices')" 
        :class="['nav-item', {active: selected === 'devices'}]"
      >
        <span class="nav-icon">🔌</span>
        <span class="nav-label">智能设备</span>
        <div class="active-indicator"></div>
      </div>
    </div>

    <div class="spacer"></div>

    <!-- 底部设置按钮 -->
    <div class="bottom-area">
      <button 
        class="setting-btn" 
        @click="$emit('select','settings')"
        :class="{active: selected === 'settings'}"
      >
        <span class="nav-icon">⚙️</span>
        <span class="nav-label">系统设置</span>
        <div class="active-indicator"></div>
      </button>
    </div>
  </aside>
</template>

<script>
export default {
  name: 'AnxSidebar',
  props: {
    selected: {
      type: String,
      default: 'info'
    }
  }
}
</script>

<style scoped>
.sidebar {
  width: 200px;
  min-height: 600px;
  display: flex;
  flex-direction: column;
  padding: 24px 16px;
  gap: 20px;
  background: linear-gradient(180deg, rgba(255, 255, 255, 0.08) 0%, rgba(255, 255, 255, 0.03) 100%);
  border-radius: 24px;
  backdrop-filter: blur(20px);
  border: 1px solid rgba(255, 255, 255, 0.12);
  box-shadow: 
    0 8px 32px rgba(0, 0, 0, 0.3),
    inset 0 1px 0 rgba(255, 255, 255, 0.1);
}

/* ===== 导航按钮区域 ===== */
.top-buttons {
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.nav-item {
  cursor: pointer;
  padding: 14px 16px;
  display: flex;
  align-items: center;
  gap: 12px;
  border-radius: 12px;
  background: rgba(255, 255, 255, 0.03);
  border: 1px solid rgba(255, 255, 255, 0.06);
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  position: relative;
  overflow: hidden;
}

.nav-item::before {
  content: '';
  position: absolute;
  left: 0;
  top: 0;
  bottom: 0;
  width: 3px;
  background: linear-gradient(180deg, #3b6eff 0%, #8b5cf6 100%);
  transform: scaleY(0);
  transition: transform 0.3s ease;
}

.nav-item:hover {
  background: rgba(255, 255, 255, 0.08);
  border-color: rgba(59, 110, 255, 0.2);
  transform: translateX(4px);
}

.nav-item:hover::before {
  transform: scaleY(0.6);
}

.nav-item.active {
  background: linear-gradient(90deg, rgba(59, 110, 255, 0.15) 0%, rgba(139, 92, 246, 0.1) 100%);
  border: 1px solid rgba(59, 110, 255, 0.4);
  box-shadow: 
    0 4px 16px rgba(59, 110, 255, 0.2),
    inset 0 0 12px rgba(59, 110, 255, 0.05);
}

.nav-item.active::before {
  transform: scaleY(1);
}

.nav-icon {
  font-size: 1.3rem;
  filter: drop-shadow(0 2px 4px rgba(0, 0, 0, 0.2));
  transition: all 0.3s ease;
}

.nav-item:hover .nav-icon {
  transform: scale(1.15);
}

.nav-item.active .nav-icon {
  filter: drop-shadow(0 0 8px rgba(59, 110, 255, 0.6));
}

.nav-label {
  flex: 1;
  font-size: 0.9rem;
  color: rgba(234, 240, 255, 0.85);
  font-weight: 600;
  letter-spacing: 0.3px;
}

.nav-item.active .nav-label {
  color: #ffffff;
  text-shadow: 0 0 10px rgba(59, 110, 255, 0.5);
}

/* 激活指示器 */
.active-indicator {
  width: 6px;
  height: 6px;
  border-radius: 50%;
  background: rgba(59, 110, 255, 0.4);
  opacity: 0;
  transition: all 0.3s ease;
}

.nav-item.active .active-indicator {
  opacity: 1;
  background: #3b6eff;
  box-shadow: 0 0 8px rgba(59, 110, 255, 0.8);
  animation: pulse 2s infinite;
}

@keyframes pulse {
  0%, 100% {
    transform: scale(1);
    opacity: 1;
  }
  50% {
    transform: scale(1.3);
    opacity: 0.7;
  }
}

/* ===== 间距占位符 ===== */
.spacer {
  flex: 1;
}

/* ===== 底部设置按钮 ===== */
.bottom-area {
  margin-top: auto;
}

.setting-btn {
  width: 100%;
  padding: 14px 16px;
  display: flex;
  align-items: center;
  gap: 12px;
  background: rgba(255, 255, 255, 0.03);
  border: 1px solid rgba(255, 255, 255, 0.06);
  border-radius: 12px;
  cursor: pointer;
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  position: relative;
  overflow: hidden;
  font-family: inherit;
}

.setting-btn::before {
  content: '';
  position: absolute;
  left: 0;
  top: 0;
  bottom: 0;
  width: 3px;
  background: linear-gradient(180deg, #f59e0b 0%, #ef4444 100%);
  transform: scaleY(0);
  transition: transform 0.3s ease;
}

.setting-btn:hover {
  background: rgba(255, 255, 255, 0.08);
  border-color: rgba(245, 158, 11, 0.3);
  transform: translateX(4px);
}

.setting-btn:hover::before {
  transform: scaleY(0.6);
}

.setting-btn.active {
  background: linear-gradient(90deg, rgba(245, 158, 11, 0.15) 0%, rgba(239, 68, 68, 0.1) 100%);
  border: 1px solid rgba(245, 158, 11, 0.4);
  box-shadow: 
    0 4px 16px rgba(245, 158, 11, 0.2),
    inset 0 0 12px rgba(245, 158, 11, 0.05);
}

.setting-btn.active::before {
  transform: scaleY(1);
}

.setting-btn .nav-icon {
  font-size: 1.2rem;
}

.setting-btn.active .nav-icon {
  filter: drop-shadow(0 0 8px rgba(245, 158, 11, 0.6));
}

.setting-btn .nav-label {
  color: rgba(234, 240, 255, 0.85);
}

.setting-btn.active .nav-label {
  color: #ffffff;
  text-shadow: 0 0 10px rgba(245, 158, 11, 0.5);
}

.setting-btn.active .active-indicator {
  background: #f59e0b;
  box-shadow: 0 0 8px rgba(245, 158, 11, 0.8);
}

/* ===== 玻璃卡片效果 ===== */
.glass-card {
  background: rgba(18, 25, 45, 0.65);
  backdrop-filter: blur(8px);
  border-radius: 12px;
  border: 1px solid rgba(72, 120, 255, 0.06);
}
</style>
