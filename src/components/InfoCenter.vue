<template>
  <div :class="['info-card', big ? 'big' : '']">
    <div class="info-header">
      <span class="header-icon">📊</span>
      <span class="header-title">信息中心</span>
      <div class="header-decoration"></div>
    </div>
    <div class="info-center-body">
      <AnxECG v-if="!mini" />
    </div>
  </div>
</template>

<script>
import AnxECG from './AnxECG.vue'

export default {
  name: 'AnxInfoCenter',
  components: { AnxECG },
  props: {
    mini: { type: Boolean, default: false },
    big: { type: Boolean, default: true }
  }
}
</script>

<style scoped>
.info-card {
  padding: 12px;
  border-radius: 16px;
  background: linear-gradient(135deg, rgba(255, 255, 255, 0.06) 0%, rgba(255, 255, 255, 0.02) 100%);
  color: #eef2ff;
  border: 1px solid rgba(255, 255, 255, 0.08);
  backdrop-filter: blur(10px);
  transition: all 0.3s ease;
}

.info-card.big {
  padding: 24px;
  min-height: 10px;
  display: flex;
  flex-direction: column;
  gap: 16px;
  background: linear-gradient(135deg, rgba(255, 255, 255, 0.08) 0%, rgba(255, 255, 255, 0.03) 100%);
  border: 1px solid rgba(255, 255, 255, 0.12);
  box-shadow: 
    0 8px 24px rgba(0, 0, 0, 0.2),
    inset 0 1px 0 rgba(255, 255, 255, 0.1);
}

.info-header {
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 12px 16px;
  position: relative;
  overflow: hidden;
}

.header-icon {
  font-size: 1.4rem;
  filter: drop-shadow(0 2px 4px rgba(0, 0, 0, 0.2));
}

.header-title {
  font-size: 1.1rem;
  font-weight: 700;
  color: #dfe8ff;
  letter-spacing: 0.5px;
}

.header-decoration {
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  height: 2px;
  background: linear-gradient(90deg, transparent 0%, #3b6eff 50%, transparent 100%);
  opacity: 0.6;
}

.info-center-body {
  flex: 1;
  display: flex;
  align-items: center;
  justify-content: center;
}

.center-list {
  display: flex;
  flex-direction: row;
  gap: 28px;
  align-items: center;
}

.center-item {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 8px;
  padding: 18px 24px;
  border-radius: 14px;
  min-width: 150px;
  background: rgba(255, 255, 255, 0.03);
  border: 1px solid rgba(255, 255, 255, 0.06);
  transition: all 0.3s ease;
}

.center-item:hover {
  background: rgba(255, 255, 255, 0.06);
  border-color: rgba(59, 110, 255, 0.2);
  transform: translateY(-3px);
  box-shadow: 0 8px 20px rgba(0, 0, 0, 0.15);
}

.ci-title {
  color: #9fb0dd;
  font-size: 0.82rem;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 0.5px;
}

.ci-value {
  color: #ffffff;
  font-weight: 800;
  font-size: 1.5rem;
  text-shadow: 0 2px 8px rgba(59, 110, 255, 0.3);
}
</style>
