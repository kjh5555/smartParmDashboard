<script setup>
import { ref } from 'vue'
import { useRouter } from 'vue-router'
import DashboardView from '../components/DashboardView.vue'
import MonitoringView from '../components/MonitoringView.vue'
import ControlView from '../components/ControlView.vue'
import AnalyticsView from '../components/AnalyticsView.vue'

const router = useRouter()

const logout = () => {
  router.push('/login')
}

const selectedFarm = ref('farm1')
const activeTab = ref('dashboard')

// YouTube 영상 ID 매핑 수정
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

const farms = {
  farm1: {
    name: '스마트팜 1호점',
    location: '경기도 화성시',
    type: '수직농장',
    area: 250, // 면적 (평)
    crops: ['상추', '딸기', '토마토'],
    data: {
      temperature: 23,
      humidity: 65,
      co2: 800,
      light: 12000,
      ec: 1.8,
      ph: 6.5,
      vpd: 1.2, // 수증기압차
      leafTemp: 22.5, // 엽온
      rootZoneTemp: 21, // 근권부 온도
      nutrientSolution: {
        no3: 12.5, // 질산염
        nh4: 1.2, // 암모늄
        p: 3.5, // 인산
        k: 6.8, // 칼륨
        ca: 8.2, // 칼슘
        mg: 3.1, // 마그네슘
      }
    },
    stats: {
      dailyHarvest: 450,
      waterUsage: 2800,
      energyUsage: 320,
      growthRate: 92,
      germination: 95, // 발아율
      fruitSet: 88, // 착과율
      nutrientEfficiency: 94, // 양분 이용 효율
      photoEfficiency: 85, // 광합성 효율
    },
    alerts: [
      { type: 'warning', message: 'EC 농도 최적범위 초과' },
      { type: 'info', message: '광량 부족 가능성' }
    ]
  },
  farm2: {
    name: '스마트팜 2호점',
    location: '충청남도 천안시',
    type: '온실',
    area: 400, // 면적 (평)
    crops: ['파프리카', '방울토마토'],
    data: {
      temperature: 25,
      humidity: 70,
      co2: 750,
      light: 15000,
      ec: 2.0,
      ph: 6.2
    },
    stats: {
      dailyHarvest: 380,
      waterUsage: 2500,
      energyUsage: 280,
      growthRate: 88
    }
  },
  farm3: {
    name: '스마트팜 3호점',
    location: '전라북도 김제시',
    type: '수경재배',
    area: 300, // 면적 (평)
    crops: ['깻잎', '바질', '루꼴라'],
    data: {
      temperature: 22,
      humidity: 75,
      co2: 720,
      light: 11000,
      ec: 1.5,
      ph: 6.8
    },
    stats: {
      dailyHarvest: 320,
      waterUsage: 2200,
      energyUsage: 250,
      growthRate: 95
    }
  }
}

const getStatusColor = (value, type) => {
  if (type === 'temperature') {
    return value < 20 ? 'text-blue-500' : value > 26 ? 'text-red-500' : 'text-green-500'
  }
  if (type === 'humidity') {
    return value < 60 ? 'text-yellow-500' : value > 80 ? 'text-red-500' : 'text-green-500'
  }
  if (type === 'vpd') {
    return value < 0.8 ? 'text-yellow-500' : value > 1.5 ? 'text-red-500' : 'text-green-500'
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
    vpd: '0.8-1.5kPa',
    leafTemp: '21-24°C',
    rootZoneTemp: '20-22°C'
  }
  return ranges[type] || ''
}
</script>

<template>
  <div class="min-h-screen bg-gray-100 py-6">
    <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
      <!-- 네비게이션 바 -->
      <nav class="bg-white shadow-sm mb-8">
        <div class="mx-auto max-w-7xl px-4 sm:px-6 lg:px-8">
          <div class="flex h-16 justify-between items-center">
            <div class="flex items-center">
              <div class="flex-shrink-0">
                <span class="text-2xl font-bold text-green-600">스마트팜</span>
              </div>
              <div class="hidden md:ml-6 md:flex md:space-x-8">
                <a href="#" 
                  @click.prevent="activeTab = 'dashboard'"
                  :class="[
                    activeTab === 'dashboard' 
                      ? 'border-green-500 text-gray-900' 
                      : 'border-transparent text-gray-500 hover:border-gray-300 hover:text-gray-700',
                    'inline-flex items-center border-b-2 px-1 pt-1 text-sm font-medium'
                  ]"
                >대시보드</a>
                <a href="#" 
                  @click.prevent="activeTab = 'monitoring'"
                  :class="[
                    activeTab === 'monitoring' 
                      ? 'border-green-500 text-gray-900' 
                      : 'border-transparent text-gray-500 hover:border-gray-300 hover:text-gray-700',
                    'inline-flex items-center border-b-2 px-1 pt-1 text-sm font-medium'
                  ]"
                >모니터링</a>
                <a href="#" 
                  @click.prevent="activeTab = 'control'"
                  :class="[
                    activeTab === 'control' 
                      ? 'border-green-500 text-gray-900' 
                      : 'border-transparent text-gray-500 hover:border-gray-300 hover:text-gray-700',
                    'inline-flex items-center border-b-2 px-1 pt-1 text-sm font-medium'
                  ]"
                >제어</a>
                <a href="#" 
                  @click.prevent="activeTab = 'analytics'"
                  :class="[
                    activeTab === 'analytics' 
                      ? 'border-green-500 text-gray-900' 
                      : 'border-transparent text-gray-500 hover:border-gray-300 hover:text-gray-700',
                    'inline-flex items-center border-b-2 px-1 pt-1 text-sm font-medium'
                  ]"
                >분석</a>
              </div>
            </div>
            <div class="flex items-center">
              <button @click="logout" class="ml-3 inline-flex items-center rounded-md bg-white px-3 py-2 text-sm font-semibold text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 hover:bg-gray-50">
                로그아웃
              </button>
            </div>
          </div>
        </div>
      </nav>

      <!-- 컴포넌트 렌더링 -->
      <component 
        :is="activeTab === 'dashboard' ? DashboardView :
             activeTab === 'monitoring' ? MonitoringView :
             activeTab === 'control' ? ControlView :
             AnalyticsView"
        v-model:selectedFarm="selectedFarm"
        :farms="farms"
      />
    </div>
  </div>
</template> 