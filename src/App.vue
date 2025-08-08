<script setup lang="ts">
import Header from './components/Header.vue'
import ServiceFrequency from './components/ServiceFrequency.vue'

const scheduleConfig = JSON.stringify({
  frequency: 'custom',
  days: ['Monday', 'Wednesday'],
  timeWindow: { start: '06:30', end: '07:30' },
  exactTime: '13:30',
})
</script>

<template>
  <Header />
  <ServiceFrequency :scheduleConfig="scheduleConfig" />
</template>

<style scoped></style>
