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
  padding: 12px;
  gap: 12px;
}
.click-item { cursor:pointer }
.click-item:hover { transform: translateY(-2px); }
.brand-overview { cursor:pointer }
.brand-overview.active, .click-item.active { box-shadow: 0 6px 20px rgba(59,110,255,0.08); border:1px solid rgba(59,110,255,0.08) }
.brand-overview { padding:10px; display:flex; flex-direction:column; gap:6px; align-items:center; text-align:center }
.brand-logo { width:56px; height:56px; border-radius:10px; background: linear-gradient(135deg,#3b6eff,#8b5cf6); display:flex; align-items:center; justify-content:center; color:white; font-weight:700 }
.brand-name { font-size:0.85rem; color:#eaf0ff; font-weight:600 }
.brand-sub { font-size:0.7rem; color:#96a9d6 }
.top-buttons { display:flex; flex-direction:column; gap:12px; }
.spacer { flex: 1 1 auto; }
.setting-btn { width:100%; }

/* 智能设备按钮样式 */
.sidebar-device-btn {
  padding: 10px 6px;
  text-align: center;
  font-size: 0.95rem;
  font-weight: 600;
  color: #cddcff;
  background: rgba(255,255,255,0.03);
  border-radius: 8px;
  transition: all 0.2s;
}

/* reuse existing outline button style */
.btn-outline { background: rgba(255,255,255,0.03); border:1px solid rgba(72,120,255,0.15); border-radius:8px; padding:8px 6px; text-align:center; font-size:0.85rem; color:#cddcff; cursor:default }

.glass-card { background: rgba(18, 25, 45, 0.65); backdrop-filter: blur(8px); border-radius:12px; border:1px solid rgba(72,120,255,0.06); }

.setting-btn:hover { background: rgba(72,120,255,0.08); }

</style>
