<script setup>
import { ref } from 'vue'

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

// 제어 설정값
const controlSettings = ref({
  temperature: 23,
  humidity: 65,
  co2: 800,
  light: 12000,
  ec: 1.8,
  ph: 6.5,
  irrigation: {
    frequency: 6,
    duration: 3,
    startTime: '06:00'
  },
  ventilation: {
    mode: 'auto',
    speed: 50
  },
  lighting: {
    mode: 'auto',
    intensity: 80,
    duration: 16,
    startTime: '06:00'
  }
})

// 재배층별 설정값
const layerSettings = ref(Array.from({ length: 9 }, (_, i) => ({
  id: i + 1,
  isActive: true,
  temperature: 23,
  humidity: 65,
  co2: 800,
  light: 12000,
  irrigation: {
    isOn: false,
    frequency: 6,
    duration: 3
  },
  lighting: {
    isOn: true,
    intensity: 80
  }
})))

// 제어 모드
const controlModes = {
  auto: '자동',
  manual: '수동'
}

// 선택된 재배층
const selectedLayer = ref(null)

// 재배층 선택 함수
const selectLayer = (layer) => {
  selectedLayer.value = layer
}

// 재배층 활성화/비활성화 토글
const toggleLayer = (layer) => {
  layer.isActive = !layer.isActive
}

// 재배층 조명 토글
const toggleLight = (layer) => {
  layer.lighting.isOn = !layer.lighting.isOn
}

// 재배층 관수 토글
const toggleIrrigation = (layer) => {
  layer.irrigation.isOn = !layer.irrigation.isOn
}
</script>

<template>
  <div class="space-y-8">
    <!-- 헤더 -->
    <div class="flex items-center space-x-3 mb-6">
      <div class="w-10 h-10 bg-green-500 rounded-lg flex items-center justify-center text-white">
        <svg class="w-6 h-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" 
            d="M12 6.253v13m0-13C10.832 5.477 9.246 5 7.5 5S4.168 5.477 3 6.253v13C4.168 18.477 5.754 18 7.5 18s3.332.477 4.5 1.253m0-13C13.168 5.477 14.754 5 16.5 5c1.747 0 3.332.477 4.5 1.253v13C19.832 18.477 18.247 18 16.5 18c-1.746 0-3.332.477-4.5 1.253" />
        </svg>
      </div>
      <div>
        <h1 class="text-2xl font-bold text-gray-900">스마트팜 제어 시스템</h1>
        <p class="text-sm text-gray-500">수직농장 환경 제어 인터페이스</p>
      </div>
    </div>

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

    <!-- 메인 제어 인터페이스 -->
    <div class="grid grid-cols-12 gap-6">
      <!-- 좌측 제어 패널 -->
      <div class="col-span-3 space-y-4">
        <div class="bg-white rounded-lg shadow p-6">
          <h3 class="text-lg font-medium text-gray-900 mb-4">전체 환경 제어</h3>
          <div class="space-y-4">
            <div>
              <label class="block text-sm font-medium text-gray-700">온도 설정 (°C)</label>
              <input type="number" v-model="controlSettings.temperature"
                class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-green-500 focus:ring-green-500 sm:text-sm"
                min="15" max="35" step="0.5"
              >
            </div>
            <div>
              <label class="block text-sm font-medium text-gray-700">습도 설정 (%)</label>
              <input type="number" v-model="controlSettings.humidity"
                class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-green-500 focus:ring-green-500 sm:text-sm"
                min="40" max="90" step="1"
              >
            </div>
            <div>
              <label class="block text-sm font-medium text-gray-700">CO2 농도 (ppm)</label>
              <input type="number" v-model="controlSettings.co2"
                class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-green-500 focus:ring-green-500 sm:text-sm"
                min="400" max="2000" step="50"
              >
            </div>
          </div>
        </div>
      </div>

      <!-- 중앙 농장 시각화 -->
      <div class="col-span-6">
        <div class="bg-white rounded-lg shadow p-6 h-full">
          <div class="relative h-[650px] bg-gray-50 rounded-lg overflow-hidden">
            <!-- 농장 시각화 -->
            <div class="absolute inset-0 flex">
              <!-- 수직 농장 레이아웃 -->
              <div class="w-full h-full p-2">
                <div class="grid grid-cols-3 grid-rows-3 gap-2 w-full h-full">
                  <div v-for="layer in layerSettings" :key="layer.id" 
                    class="bg-white rounded-lg relative border border-gray-200 cursor-pointer hover:bg-gray-50 transition-colors p-3"
                    :class="[
                      layer.isActive ? '' : 'opacity-60',
                      selectedLayer?.id === layer.id ? 'ring-2 ring-green-500' : ''
                    ]"
                    @click="selectLayer(layer)"
                  >
                    <div class="flex flex-col h-full">
                      <div class="flex justify-between items-start">
                        <span class="text-base font-medium text-gray-900">
                          재배층{{ layer.id }}
                        </span>
                        <button 
                          @click.stop="toggleLayer(layer)"
                          class="p-1 rounded-full hover:bg-gray-100"
                          :class="layer.isActive ? 'text-green-600' : 'text-gray-400'"
                        >
                          <svg class="w-5 h-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 10V3L4 14h7v7l9-11h-7z" />
                          </svg>
                        </button>
                      </div>
                      
                      <div class="flex flex-col space-y-2 flex-grow mt-2">
                        <div class="flex justify-between items-center">
                          <span class="text-sm text-gray-500">온도</span>
                          <span class="text-sm text-gray-900">{{ layer.temperature }}°C</span>
                        </div>
                        <div class="flex justify-between items-center">
                          <span class="text-sm text-gray-500">습도</span>
                          <span class="text-sm text-gray-900">{{ layer.humidity }}%</span>
                        </div>
                      </div>

                      <div class="flex justify-between items-center mt-2">
                        <button 
                          @click.stop="toggleLight(layer)"
                          class="p-1 rounded hover:bg-gray-100"
                          :class="layer.lighting.isOn ? 'text-yellow-500' : 'text-gray-400'"
                        >
                          <svg class="w-5 h-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9.663 17h4.673M12 3v1m6.364 1.636l-.707.707M21 12h-1M4 12H3m3.343-5.657l-.707-.707m2.828 9.9a5 5 0 117.072 0l-.548.547A3.374 3.374 0 0014 18.469V19a2 2 0 11-4 0v-.531c0-.895-.356-1.754-.988-2.386l-.548-.547z" />
                          </svg>
                        </button>
                        <button 
                          @click.stop="toggleIrrigation(layer)"
                          class="p-1 rounded hover:bg-gray-100"
                          :class="layer.irrigation.isOn ? 'text-blue-500' : 'text-gray-400'"
                        >
                          <svg class="w-5 h-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19.428 15.428a2 2 0 00-1.022-.547l-2.387-.477a6 6 0 00-3.86.517l-.318.158a6 6 0 01-3.86.517L6.05 15.21a2 2 0 00-1.806.547M8 4h8l-1 1v5.172a2 2 0 00.586 1.414l5 5c1.26 1.26.367 3.414-1.415 3.414H4.828c-1.782 0-2.674-2.154-1.414-3.414l5-5A2 2 0 009 10.172V5L8 4z" />
                          </svg>
                        </button>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>

      <!-- 우측 제어 패널 -->
      <div class="col-span-3 space-y-4">
        <div v-if="selectedLayer" class="bg-white rounded-lg shadow p-6">
          <div class="flex justify-between items-center mb-4">
            <h3 class="text-lg font-medium text-gray-900">재배층 {{ selectedLayer.id }} 제어</h3>
            <span :class="selectedLayer.isActive ? 'text-green-500' : 'text-gray-500'">
              {{ selectedLayer.isActive ? '가동중' : '중지됨' }}
            </span>
          </div>
          <div class="space-y-4">
            <div>
              <label class="block text-sm font-medium text-gray-700">온도 설정 (°C)</label>
              <input type="number" v-model="selectedLayer.temperature"
                :disabled="!selectedLayer.isActive"
                class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-green-500 focus:ring-green-500 sm:text-sm"
                min="15" max="35" step="0.5"
              >
            </div>
            <div>
              <label class="block text-sm font-medium text-gray-700">습도 설정 (%)</label>
              <input type="number" v-model="selectedLayer.humidity"
                :disabled="!selectedLayer.isActive"
                class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-green-500 focus:ring-green-500 sm:text-sm"
                min="40" max="90" step="1"
              >
            </div>
            <div>
              <label class="block text-sm font-medium text-gray-700">광도 설정 (%)</label>
              <input type="range" v-model="selectedLayer.lighting.intensity"
                :disabled="!selectedLayer.isActive || !selectedLayer.lighting.isOn"
                class="mt-1 block w-full"
                min="0" max="100" step="5"
              >
              <div class="mt-1 flex justify-between text-xs text-gray-500">
                <span>0%</span>
                <span>{{ selectedLayer.lighting.intensity }}%</span>
                <span>100%</span>
              </div>
            </div>
            <div>
              <label class="block text-sm font-medium text-gray-700">관수 주기 (회/일)</label>
              <input type="number" v-model="selectedLayer.irrigation.frequency"
                :disabled="!selectedLayer.isActive || !selectedLayer.irrigation.isOn"
                class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-green-500 focus:ring-green-500 sm:text-sm"
                min="1" max="12" step="1"
              >
            </div>
          </div>
        </div>
        <div v-else class="bg-white rounded-lg shadow p-6">
          <p class="text-gray-500 text-center">재배층을 선택하여 개별 제어하세요</p>
        </div>
      </div>
    </div>
  </div>
</template> 