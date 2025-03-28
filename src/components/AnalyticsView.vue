<script setup>
const props = defineProps({
  selectedFarm: {
    type: String,
    required: true
  },
  farms: {
    type: Object,
    required: true
  }
})

const emit = defineEmits(['update:selectedFarm'])
</script>

<template>
  <div class="space-y-8">
    <!-- 농장 선택 -->
    <div class="bg-white rounded-lg shadow p-6">
      <h3 class="text-sm font-medium text-gray-700 mb-2">농장 선택</h3>
      <select
        :value="selectedFarm"
        @input="emit('update:selectedFarm', $event.target.value)"
        class="mt-1 block w-full pl-3 pr-10 py-2 text-base border-gray-300 focus:outline-none focus:ring-green-500 focus:border-green-500 sm:text-sm rounded-md"
      >
        <option v-for="(farm, id) in farms" :key="id" :value="id">
          {{ farm.name }} ({{ farm.location }})
        </option>
      </select>
    </div>

    <!-- 분석 대시보드 -->
    <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
      <!-- 생산성 분석 -->
      <div class="bg-white rounded-lg shadow p-6">
        <h3 class="text-lg font-medium text-gray-900 mb-4">생산성 분석</h3>
        <div class="space-y-4">
          <div class="flex justify-between items-center">
            <span class="text-sm text-gray-500">일일 수확량</span>
            <span class="text-lg font-medium text-gray-900">{{ farms[selectedFarm].stats.dailyHarvest }}kg</span>
          </div>
          <div class="flex justify-between items-center">
            <span class="text-sm text-gray-500">물 사용량</span>
            <span class="text-lg font-medium text-gray-900">{{ farms[selectedFarm].stats.waterUsage }}L</span>
          </div>
          <div class="flex justify-between items-center">
            <span class="text-sm text-gray-500">에너지 사용량</span>
            <span class="text-lg font-medium text-gray-900">{{ farms[selectedFarm].stats.energyUsage }}kWh</span>
          </div>
          <div class="flex justify-between items-center">
            <span class="text-sm text-gray-500">생장률</span>
            <span class="text-lg font-medium text-gray-900">{{ farms[selectedFarm].stats.growthRate }}%</span>
          </div>
        </div>
      </div>

      <!-- 환경 분석 -->
      <div class="bg-white rounded-lg shadow p-6">
        <h3 class="text-lg font-medium text-gray-900 mb-4">환경 분석</h3>
        <div class="space-y-4">
          <div class="flex justify-between items-center">
            <span class="text-sm text-gray-500">평균 온도</span>
            <span class="text-lg font-medium text-gray-900">{{ farms[selectedFarm].data.temperature }}°C</span>
          </div>
          <div class="flex justify-between items-center">
            <span class="text-sm text-gray-500">평균 습도</span>
            <span class="text-lg font-medium text-gray-900">{{ farms[selectedFarm].data.humidity }}%</span>
          </div>
          <div class="flex justify-between items-center">
            <span class="text-sm text-gray-500">CO2 농도</span>
            <span class="text-lg font-medium text-gray-900">{{ farms[selectedFarm].data.co2 }}ppm</span>
          </div>
          <div class="flex justify-between items-center">
            <span class="text-sm text-gray-500">광량</span>
            <span class="text-lg font-medium text-gray-900">{{ farms[selectedFarm].data.light }}lux</span>
          </div>
        </div>
      </div>

      <!-- 양액 분석 -->
      <div class="bg-white rounded-lg shadow p-6">
        <h3 class="text-lg font-medium text-gray-900 mb-4">양액 분석</h3>
        <div class="grid grid-cols-3 gap-4">
          <div v-for="(value, nutrient) in farms[selectedFarm].data.nutrientSolution" :key="nutrient"
            class="bg-gray-50 rounded-lg p-4"
          >
            <p class="text-sm text-gray-500">{{ nutrient.toUpperCase() }}</p>
            <p class="text-lg font-medium text-gray-900">{{ value }}mg/L</p>
          </div>
        </div>
      </div>

      <!-- 생육 상태 분석 -->
      <div class="bg-white rounded-lg shadow p-6">
        <h3 class="text-lg font-medium text-gray-900 mb-4">생육 상태 분석</h3>
        <div class="space-y-4">
          <div class="flex justify-between items-center">
            <span class="text-sm text-gray-500">발아율</span>
            <span class="text-lg font-medium text-gray-900">{{ farms[selectedFarm].stats.germination }}%</span>
          </div>
          <div class="flex justify-between items-center">
            <span class="text-sm text-gray-500">착과율</span>
            <span class="text-lg font-medium text-gray-900">{{ farms[selectedFarm].stats.fruitSet }}%</span>
          </div>
          <div class="flex justify-between items-center">
            <span class="text-sm text-gray-500">양분 이용 효율</span>
            <span class="text-lg font-medium text-gray-900">{{ farms[selectedFarm].stats.nutrientEfficiency }}%</span>
          </div>
          <div class="flex justify-between items-center">
            <span class="text-sm text-gray-500">광합성 효율</span>
            <span class="text-lg font-medium text-gray-900">{{ farms[selectedFarm].stats.photoEfficiency }}%</span>
          </div>
        </div>
      </div>
    </div>
  </div>
</template> 