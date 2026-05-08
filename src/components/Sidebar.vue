<template>
  <aside class="sidebar glass-card">
    <div class="brand-overview" role="button" tabindex="0" @click="$emit('select','brand')" :class="{active: selected === 'brand'}">
      <div class="brand-logo">安芯居</div>
      <div class="brand-name">安芯居 · 家庭守护</div>
      <div class="brand-sub">设备/状态总览</div>
    </div>

    <div class="top-buttons">
      <div @click="$emit('select','info')" :class="['click-item', {active: selected === 'info'}]">
        <InfoCenter :mini="true" />
      </div>
      <div @click="$emit('select','devices')" :class="['click-item', {active: selected === 'devices'}]">
        <div class="sidebar-device-btn">智能设备</div>
      </div>
    </div>

    <div class="spacer"></div>

    <div class="bottom-area">
      <button class="btn-outline setting-btn" @click="$emit('select','settings')">设置</button>
    </div>
  </aside>
</template>

<script>
import InfoCenter from './InfoCenter.vue'

export default {
  name: 'AnxSidebar',
  components: { InfoCenter },
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
  width: 140px;
  min-height: 600px;
  display: flex;
  flex-direction: column;
  padding: 16px;
  gap: 16px;
  background: linear-gradient(180deg, rgba(18, 25, 45, 0.8), rgba(8, 14, 26, 0.8));
  border-radius: 20px;
  border: 1px solid rgba(72, 120, 255, 0.2);
  box-shadow: 0 8px 24px rgba(0, 0, 0, 0.3);
}
.click-item {
  cursor: pointer;
  transition: all 0.3s ease;
}
.click-item:hover {
  transform: translateY(-3px);
}
.brand-overview {
  cursor: pointer;
  transition: all 0.3s ease;
}
.brand-overview.active,
.click-item.active {
  box-shadow: 0 8px 24px rgba(59, 110, 255, 0.15);
  border: 1px solid rgba(59, 110, 255, 0.2);
  background: linear-gradient(135deg, rgba(59, 110, 255, 0.1), rgba(139, 92, 246, 0.1));
}
.brand-overview {
  padding: 12px;
  display: flex;
  flex-direction: column;
  gap: 8px;
  align-items: center;
  text-align: center;
  border-radius: 16px;
}
.brand-logo {
  width: 60px;
  height: 60px;
  border-radius: 12px;
  background: linear-gradient(135deg, #3b6eff, #8b5cf6);
  display: flex;
  align-items: center;
  justify-content: center;
  color: white;
  font-weight: 700;
  font-size: 1.2rem;
  box-shadow: 0 6px 16px rgba(59, 110, 255, 0.3);
}
.brand-name {
  font-size: 0.9rem;
  color: #eaf0ff;
  font-weight: 600;
}
.brand-sub {
  font-size: 0.75rem;
  color: #96a9d6;
}
.top-buttons {
  display: flex;
  flex-direction: column;
  gap: 12px;
}
.spacer {
  flex: 1 1 auto;
}
.setting-btn {
  width: 100%;
  transition: all 0.3s ease;
}
.setting-btn:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 16px rgba(59, 110, 255, 0.2);
}

/* 智能设备按钮样式 */
.sidebar-device-btn {
  padding: 12px 8px;
  text-align: center;
  font-size: 1rem;
  font-weight: 600;
  color: #cddcff;
  background: linear-gradient(135deg, rgba(255, 255, 255, 0.06), rgba(255, 255, 255, 0.02));
  border-radius: 12px;
  transition: all 0.3s ease;
  border: 1px solid rgba(72, 120, 255, 0.1);
}

/* reuse existing outline button style */
.btn-outline {
  background: linear-gradient(135deg, rgba(255, 255, 255, 0.06), rgba(255, 255, 255, 0.02));
  border: 1px solid rgba(72, 120, 255, 0.2);
  border-radius: 12px;
  padding: 10px 8px;
  text-align: center;
  font-size: 0.9rem;
  color: #cddcff;
  cursor: pointer;
  transition: all 0.3s ease;
  font-weight: 500;
}

.glass-card {
  background: rgba(18, 25, 45, 0.75);
  backdrop-filter: blur(12px);
  border-radius: 16px;
  border: 1px solid rgba(72, 120, 255, 0.15);
}

.setting-btn:hover {
  background: linear-gradient(135deg, rgba(72, 120, 255, 0.2), rgba(139, 92, 246, 0.2));
  border-color: rgba(72, 120, 255, 0.4);
}

</style>
