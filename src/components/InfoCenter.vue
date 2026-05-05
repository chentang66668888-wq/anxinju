<template>
  <div :class="['info-card', big ? 'big' : '']">
    <div class="info-title">信息中心</div>
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
.info-card { padding:8px; border-radius:10px; background: rgba(255,255,255,0.02); color:#eef2ff; }
.info-card.big { padding:18px; min-height:180px; display:flex; flex-direction:column; gap:12px }
.info-title { font-size:1rem; font-weight:700; padding:6px 8px; color:#dfe8ff }
.info-center-body { flex:1; display:flex; align-items:center; justify-content:center }
.center-list { display:flex; flex-direction:row; gap:28px; align-items:center }
.center-item { display:flex; flex-direction:column; align-items:center; gap:6px; padding:14px 18px; border-radius:12px; min-width:140px; background: rgba(255,255,255,0.02); }
.ci-title { color:#9fb0dd; font-size:0.78rem }
.ci-value { color:#ffffff; font-weight:800; font-size:1.4rem }
</style>
