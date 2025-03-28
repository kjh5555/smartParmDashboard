<template>
  <div class="flex items-center space-x-2">
    <div class="relative">
      <div class="w-10 h-10 bg-gradient-to-br from-purple-500 to-purple-600 rounded-lg flex items-center justify-center overflow-hidden relative">
        <!-- 모니터링을 상징하는 파형 패턴 -->
        <div class="absolute inset-0 flex items-center justify-center">
          <div class="w-full h-4 flex items-center">
            <div class="w-full h-0.5 bg-white/30"></div>
            <div class="w-1 h-4 bg-white/40 rounded mx-0.5"></div>
            <div class="w-1 h-3 bg-white/40 rounded mx-0.5"></div>
            <div class="w-1 h-2 bg-white/40 rounded mx-0.5"></div>
            <div class="w-1 h-3 bg-white/40 rounded mx-0.5"></div>
            <div class="w-1 h-4 bg-white/40 rounded mx-0.5"></div>
          </div>
        </div>
        <!-- 모니터링 아이콘 -->
        <svg class="w-6 h-6 text-white relative z-10" fill="none" viewBox="0 0 24 24" stroke="currentColor">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 19v-6a2 2 0 00-2-2H5a2 2 0 00-2 2v6a2 2 0 002 2h2a2 2 0 002-2zm0 0V9a2 2 0 012-2h2a2 2 0 012 2v10m-6 0a2 2 0 002 2h2a2 2 0 002-2m0 0V5a2 2 0 012-2h2a2 2 0 012 2v14a2 2 0 01-2 2h-2a2 2 0 01-2-2z M3 4h18M3 12h18M3 20h18" />
        </svg>
      </div>
    </div>
    <div v-if="showText" class="flex flex-col">
      <span class="text-lg font-bold text-gray-900 leading-tight">모니터링</span>
      <span class="text-xs text-gray-500">Monitoring</span>
    </div>
  </div>
</template>

<script setup lang="ts">
defineProps({
  showText: {
    type: Boolean,
    default: true
  }
});
</script>

<script lang="ts">
export default {
  name: 'MonitoringIcon'
};
</script> 