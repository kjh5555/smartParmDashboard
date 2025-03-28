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

const getStatusColor = (value, type) => {
  if (type === 'temperature') {
    return value < 20 ? 'text-blue-500' : value > 26 ? 'text-red-500' : 'text-green-500'
  }
  if (type === 'humidity') {
    return value < 60 ? 'text-yellow-500' : value > 80 ? 'text-red-500' : 'text-green-500'
  }
  return 'text-green-500'
}

const getOptimalRange = (type) => {
  const ranges = {
    temperature: '20-26°C',
    humidity: '60-80%',
    co2: '800-1200ppm',
    ec: '1.5-2.5mS/cm',
    ph: '5.8-6.5',
    vpd: '0.8-1.5kPa'
  }
  return ranges[type] || ''
}

// YouTube 영상 ID 매핑
const farmVideos = {
  farm1: {
    main: 'NkdQB2ykUXM',
    zone1: 'yZvTaWJqJcg',
    zone2: 'yI-oZmf-N8c',
    zone3: 'lUKlWvH1Q0Q',
    zone4: '4L6HhgTarks',
    zone5: 'yI-oZmf-N8c',
    zone6: 'lUKlWvH1Q0Q',
    zone7: '4L6HhgTarks'
  },
  farm2: {
    main: 'kYWXJMQYDHM',
    zone1: 'yZvTaWJqJcg',
    zone2: 'yI-oZmf-N8c',
    zone3: 'lUKlWvH1Q0Q',
    zone4: '4L6HhgTarks',
    zone5: 'yI-oZmf-N8c',
    zone6: 'lUKlWvH1Q0Q',
    zone7: '4L6HhgTarks'
  },
  farm3: {
    main: 'qnm2HH8iyRY',
    zone1: 'yZvTaWJqJcg',
    zone2: 'yI-oZmf-N8c',
    zone3: 'lUKlWvH1Q0Q',
    zone4: '4L6HhgTarks',
    zone5: 'yI-oZmf-N8c',
    zone6: 'lUKlWvH1Q0Q',
    zone7: '4L6HhgTarks'
  }
}
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

    <!-- 실시간 영상 -->
    <div class="bg-white rounded-lg shadow p-6">
      <div class="flex justify-between items-center mb-4">
        <h3 class="text-lg font-medium text-gray-900">실시간 모니터링</h3>
        <span class="text-sm text-gray-500">{{ farms[selectedFarm].name }}</span>
      </div>
      <div class="grid grid-cols-4 gap-4">
        <!-- 메인 영상 (더 큰 크기) -->
        <div class="col-span-2 row-span-2">
          <div class="relative">
            <iframe
              :src="`https://www.youtube.com/embed/${farmVideos[selectedFarm].main}?autoplay=0&mute=1`"
              class="w-full h-[400px] rounded-lg"
              frameborder="0"
              allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
              allowfullscreen
            ></iframe>
            <div class="absolute top-2 left-2 bg-black bg-opacity-60 text-white px-2 py-1 rounded text-sm">
              메인 구역
            </div>
          </div>
        </div>
        <!-- 작은 영상들 -->
        <div v-for="n in 7" :key="n" class="relative">
          <iframe
            :src="`https://www.youtube.com/embed/${farmVideos[selectedFarm]['zone' + n]}?autoplay=0&mute=1`"
            class="w-full h-[190px] rounded-lg"
            frameborder="0"
            allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
            allowfullscreen
          ></iframe>
          <div class="absolute top-2 left-2 bg-black bg-opacity-60 text-white px-2 py-1 rounded text-sm">
            구역 {{ n }}
          </div>
        </div>
      </div>
      <p class="mt-4 text-sm text-gray-500">* 실제 농장의 모습과 다를 수 있습니다.</p>
    </div>

    <!-- 실시간 환경 데이터 -->
    <div class="bg-white rounded-lg shadow p-6">
      <h3 class="text-lg font-medium text-gray-900 mb-4">실시간 환경 데이터</h3>
      <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
        <div v-for="key in ['temperature', 'humidity', 'co2', 'vpd', 'ec', 'ph']" :key="key"
          class="bg-gray-50 rounded-lg p-4"
        >
          <div class="flex items-center justify-between">
            <div>
              <h4 class="text-sm font-medium text-gray-900">
                {{ key.charAt(0).toUpperCase() + key.slice(1) }}
              </h4>
              <span class="text-xs text-gray-500">적정범위: {{ getOptimalRange(key) }}</span>
            </div>
            <span :class="getStatusColor(farms[selectedFarm].data[key], key)" class="text-2xl font-bold">
              {{ farms[selectedFarm].data[key] }}
              {{ key === 'temperature' ? '°C' : key === 'humidity' ? '%' : key === 'co2' ? 'ppm' : 
                 key === 'ec' ? 'mS/cm' : key === 'vpd' ? 'kPa' : '' }}
            </span>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>