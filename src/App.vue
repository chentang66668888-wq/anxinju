<template>
  <Dashboard />
</template>

<script>
import Dashboard from './components/Dashboard.vue'

export default {
  name: 'App',
  components: { Dashboard }
}
</script>

<style>
/* keep app minimal; dashboard styles are in Dashboard.vue */
html, body, #app { height: 100%; }
</style>
