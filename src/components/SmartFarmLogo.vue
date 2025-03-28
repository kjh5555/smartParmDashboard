<template>
  <div class="flex items-center space-x-2">
    <div class="relative">
      <!-- 수직 농장을 상징하는 로고 -->
      <div class="w-10 h-10 bg-gradient-to-br from-green-500 to-green-600 rounded-lg flex items-center justify-center overflow-hidden relative">
        <!-- 수직 레이어를 상징하는 요소들 -->
        <div class="absolute inset-0 flex flex-col justify-between py-2">
          <div class="h-1 mx-2 bg-white/30 rounded"></div>
          <div class="h-1 mx-2 bg-white/30 rounded"></div>
          <div class="h-1 mx-2 bg-white/30 rounded"></div>
        </div>
        <!-- 식물 성장을 상징하는 아이콘 -->
        <svg class="w-6 h-6 text-white relative z-10" fill="none" viewBox="0 0 24 24" stroke="currentColor">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" 
            d="M12 6.253v13m0-13C10.832 5.477 9.246 5 7.5 5S4.168 5.477 3 6.253v13C4.168 18.477 5.754 18 7.5 18s3.332.477 4.5 1.253m0-13C13.168 5.477 14.754 5 16.5 5c1.747 0 3.332.477 4.5 1.253v13C19.832 18.477 18.247 18 16.5 18c-1.746 0-3.332.477-4.5 1.253" />
        </svg>
      </div>
    </div>
    <div v-if="showText" class="flex flex-col">
      <span class="text-lg font-bold text-gray-900 leading-tight">스마트팜</span>
      <span class="text-xs text-gray-500">Smart Farm</span>
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
  name: 'SmartFarmLogo'
};
</script> 