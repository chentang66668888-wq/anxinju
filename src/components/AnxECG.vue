<template>
  <div class="ecg-card glass-card">
    <div class="ecg-title">心电图</div>
    <div class="ecg-plot"><!-- placeholder for ECG chart -->
      <div class="ecg-center">心电图区域（示意）</div>
    </div>
  </div>
</template>

<script>
export default { name: 'AnxECG' }
</script>

<style scoped>
.ecg-card { padding:18px; display:flex; flex-direction:column; gap:12px; min-height:420px; }
.ecg-title { font-size:1.2rem; color:#dfe8ff; font-weight:600; }
.ecg-plot { flex:1; background: linear-gradient(180deg, rgba(10,18,35,0.12), rgba(5,10,20,0.06)); border-radius:12px; display:flex; align-items:center; justify-content:center; color:#9fb4ff; font-size:1.6rem; }
.ecg-center { opacity:0.9 }
</style>
