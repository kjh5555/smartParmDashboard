<script setup>
defineProps({
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

const getStatusColor = (value, type) => {
  if (type === 'temperature') {
    return value < 20 ? 'text-blue-500' : value > 26 ? 'text-red-500' : 'text-green-500'
  }
  if (type === 'humidity') {
    return value < 60 ? 'text-yellow-500' : value > 80 ? 'text-red-500' : 'text-green-500'
  }
  return 'text-green-500'
}
</script>

<template>
  <div class="space-y-8">
    <!-- 농장 선택 및 알림 섹션 -->
    <div class="mb-8 grid grid-cols-1 md:grid-cols-2 gap-6">
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
      <div class="bg-white rounded-lg shadow p-6">
        <h3 class="text-sm font-medium text-gray-700 mb-2">실시간 알림</h3>
        <div v-if="farms[selectedFarm].alerts?.length" class="space-y-2">
          <div v-for="(alert, index) in farms[selectedFarm].alerts" :key="index"
            :class="{'bg-yellow-50': alert.type === 'warning', 'bg-blue-50': alert.type === 'info'}"
            class="p-3 rounded-md text-sm flex items-center"
          >
            <span v-if="alert.type === 'warning'" class="mr-2">⚠️</span>
            <span v-else class="mr-2">ℹ️</span>
            <span :class="{'text-yellow-800': alert.type === 'warning', 'text-blue-800': alert.type === 'info'}">
              {{ alert.message }}
            </span>
          </div>
        </div>
        <div v-else class="text-gray-500 text-sm p-3 bg-gray-50 rounded-md">
          현재 활성화된 알림이 없습니다.
        </div>
      </div>
    </div>

    <!-- 농장 정보 카드 -->
    <div class="bg-white rounded-lg shadow px-5 py-6 sm:px-6">
      <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
        <div>
          <h2 class="text-2xl font-bold text-gray-900 mb-4">{{ farms[selectedFarm].name }}</h2>
          <p class="text-gray-600">위치: {{ farms[selectedFarm].location }}</p>
          <p class="text-gray-600">재배 방식: {{ farms[selectedFarm].type }}</p>
          <p class="text-gray-600">면적: {{ farms[selectedFarm].area }}평</p>
          <p class="text-gray-600">재배 작물: {{ farms[selectedFarm].crops.join(', ') }}</p>
        </div>
        <div class="grid grid-cols-2 gap-4">
          <div class="bg-green-50 rounded-lg p-4">
            <p class="text-sm text-gray-500">발아율</p>
            <p class="text-2xl font-bold text-green-600">{{ farms[selectedFarm].stats.germination }}%</p>
          </div>
          <div class="bg-blue-50 rounded-lg p-4">
            <p class="text-sm text-gray-500">착과율</p>
            <p class="text-2xl font-bold text-blue-600">{{ farms[selectedFarm].stats.fruitSet }}%</p>
          </div>
        </div>
      </div>
    </div>

    <!-- 환경 데이터 그리드 -->
    <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
      <div v-for="key in ['temperature', 'humidity', 'co2', 'vpd', 'ec', 'ph']" :key="key"
        class="bg-white rounded-lg shadow px-5 py-6"
      >
        <div class="flex items-center justify-between">
          <h3 class="text-lg font-medium text-gray-900">
            {{ key.charAt(0).toUpperCase() + key.slice(1) }}
          </h3>
          <span :class="getStatusColor(farms[selectedFarm].data[key], key)" class="text-2xl font-bold">
            {{ farms[selectedFarm].data[key] }}
            {{ key === 'temperature' ? '°C' : key === 'humidity' ? '%' : key === 'co2' ? 'ppm' : 
               key === 'ec' ? 'mS/cm' : key === 'vpd' ? 'kPa' : '' }}
          </span>
        </div>
      </div>
    </div>

    <!-- 생산성 지표 -->
    <div class="bg-white rounded-lg shadow px-5 py-6">
      <h3 class="text-lg font-medium text-gray-900 mb-4">생산성 지표</h3>
      <div class="grid grid-cols-1 md:grid-cols-4 gap-6">
        <div class="bg-green-50 rounded-lg p-4">
          <p class="text-sm text-gray-500">일일 수확량</p>
          <p class="text-2xl font-bold text-green-600">{{ farms[selectedFarm].stats.dailyHarvest }}kg</p>
        </div>
        <div class="bg-blue-50 rounded-lg p-4">
          <p class="text-sm text-gray-500">양분 이용 효율</p>
          <p class="text-2xl font-bold text-blue-600">{{ farms[selectedFarm].stats.nutrientEfficiency }}%</p>
        </div>
        <div class="bg-yellow-50 rounded-lg p-4">
          <p class="text-sm text-gray-500">광합성 효율</p>
          <p class="text-2xl font-bold text-yellow-600">{{ farms[selectedFarm].stats.photoEfficiency }}%</p>
        </div>
        <div class="bg-purple-50 rounded-lg p-4">
          <p class="text-sm text-gray-500">생장률</p>
          <p class="text-2xl font-bold text-purple-600">{{ farms[selectedFarm].stats.growthRate }}%</p>
        </div>
      </div>
    </div>
  </div>
</template> 