<template>
  <div class="space-y-8">
    <!-- 헤더 -->
    <div class="flex items-center justify-between mb-6">
      <div class="flex items-center space-x-4">
        <MonitoringIcon />
        <div>
          <h1 class="text-2xl font-bold text-gray-900">모니터링</h1>
          <p class="text-sm text-gray-500">실시간 환경 데이터 모니터링</p>
        </div>
      </div>
      <SmartFarmLogo />
    </div>
    
    <!-- 모니터링 내용 -->
    <!-- ... existing content ... -->
  </div>
</template>

<script setup lang="ts">
import MonitoringIcon from '@/components/MonitoringIcon.vue';
import SmartFarmLogo from '@/components/SmartFarmLogo.vue';
// ... rest of the imports ...
</script> 