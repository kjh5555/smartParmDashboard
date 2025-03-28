<script setup lang="ts">
import { ref } from 'vue'
import { useRouter } from 'vue-router'

const router = useRouter()

const scrollToInfo = () => {
  const element = document.getElementById('smartfarm-info')
  element?.scrollIntoView({ behavior: 'smooth' })
}
</script>

<template>
  <div class="min-h-screen bg-gray-50">
    <!-- 메인 콘텐츠 -->
    <main>
      <div class="relative min-h-screen bg-white">
        <!-- 히어로 섹션 -->
        <div class="relative h-screen flex items-center justify-center overflow-hidden">
          <!-- 배경 이미지 및 오버레이 -->
          <div class="absolute inset-0">
            <img
              src="https://images.unsplash.com/photo-1560493676-04071c5f467b"
              class="h-full w-full object-cover"
              alt="스마트팜 배경"
            />
            <div class="absolute inset-0 bg-green-600/30 mix-blend-multiply"></div>
          </div>

          <!-- 메인 콘텐츠 -->
          <div class="relative max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 text-center">
            <h1 class="text-4xl sm:text-5xl lg:text-6xl font-bold text-white mb-8">
              미래 농업의 혁신,<br />스마트팜
            </h1>
            <p class="text-xl sm:text-2xl text-white mb-12 max-w-3xl mx-auto">
              ICT 기술로 만드는 지속 가능한 농업의 미래
            </p>
            <div class="space-y-4">
              <button
                @click="scrollToInfo"
                class="w-full sm:w-40 inline-flex items-center justify-center px-6 py-3 border border-transparent text-base font-medium rounded-md text-green-700 bg-white hover:bg-green-50 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-green-500"
              >
                더 알아보기
                <svg class="ml-2 -mr-1 h-5 w-5" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" fill="currentColor">
                  <path fill-rule="evenodd" d="M5.293 7.293a1 1 0 011.414 0L10 10.586l3.293-3.293a1 1 0 111.414 1.414l-4 4a1 1 0 01-1.414 0l-4-4a1 1 0 010-1.414z" clip-rule="evenodd" />
                </svg>
              </button>
              <div>
                <router-link
                  to="/login"
                  class="w-full sm:w-40 inline-flex items-center justify-center px-6 py-3 border border-transparent text-base font-medium rounded-md text-white bg-green-600 hover:bg-green-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-green-500"
                >
                  시작하기
                </router-link>
              </div>
            </div>
          </div>
        </div>

        <!-- 스마트팜 설명 섹션 -->
        <div id="smartfarm-info" class="py-16 bg-white">
          <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
            <div class="text-center">
              <h2 class="text-3xl font-bold text-gray-900 sm:text-4xl mb-8">
                스마트팜이란?
              </h2>
            </div>

            <div class="mt-12 grid grid-cols-1 gap-16 lg:grid-cols-2">
              <!-- 설명 섹션 -->
              <div class="space-y-6">
                <div class="prose prose-green max-w-none">
                  <p class="text-lg text-gray-700">
                    스마트팜은 센서, 정보통신기술(ICT), 기기제어, 빅데이터 등으로 복합환경제어를 자동으로 할 수 있는 시설농업기술입니다.
                  </p>
                  <h3 class="text-xl font-semibold text-gray-900 mt-8">주요 특징</h3>
                  <ul class="mt-4 space-y-4">
                    <li class="flex items-start">
                      <span class="flex-shrink-0 h-6 w-6 text-green-500">
                        <svg class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7" />
                        </svg>
                      </span>
                      <span class="ml-3 text-gray-700">
                        <strong>자동화 시스템:</strong> 로봇, 드론, 자율주행 등 첨단 기술 활용
                      </span>
                    </li>
                    <li class="flex items-start">
                      <span class="flex-shrink-0 h-6 w-6 text-green-500">
                        <svg class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7" />
                        </svg>
                      </span>
                      <span class="ml-3 text-gray-700">
                        <strong>생산성 향상:</strong> 토마토 13.7%, 딸기 30% 생산성 증가
                      </span>
                    </li>
                    <li class="flex items-start">
                      <span class="flex-shrink-0 h-6 w-6 text-green-500">
                        <svg class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7" />
                        </svg>
                      </span>
                      <span class="ml-3 text-gray-700">
                        <strong>노동력 절감:</strong> 자율주행 장비로 50% 이상 노동력 감소
                      </span>
                    </li>
                  </ul>
                </div>
              </div>

              <!-- 이미지 섹션 -->
              <div class="grid grid-cols-2 gap-4">
                <div class="space-y-4">
                  <img
                    class="rounded-lg shadow-lg object-cover h-48 w-full"
                    src="https://images.unsplash.com/photo-1530836369250-ef72a3f5cda8"
                    alt="스마트팜 내부"
                  />
                  <img
                    class="rounded-lg shadow-lg object-cover h-48 w-full"
                    src="https://images.unsplash.com/photo-1466692476868-aef1dfb1e735"
                    alt="수직농장"
                  />
                </div>
                <div class="space-y-4 pt-8">
                  <img
                    class="rounded-lg shadow-lg object-cover h-48 w-full"
                    src="https://images.unsplash.com/photo-1495107334309-fcf20504a5ab"
                    alt="자동화 시스템"
                  />
                  <img
                    class="rounded-lg shadow-lg object-cover h-48 w-full"
                    src="https://images.unsplash.com/photo-1558449028-b53a39d100fc"
                    alt="수경재배"
                  />
                </div>
              </div>
            </div>

            <!-- 발전 단계 섹션 -->
            <div class="mt-16">
              <h3 class="text-2xl font-bold text-gray-900 mb-8 text-center">
                스마트팜의 발전 단계
              </h3>
              <div class="grid grid-cols-1 md:grid-cols-3 gap-8">
                <div class="bg-white p-6 rounded-lg shadow-lg border border-green-100">
                  <div class="text-green-600 text-xl font-semibold mb-4">1세대</div>
                  <p class="text-gray-600">
                    ICT 기술을 활용한 시설 환경정보 모니터링 및 원격 제어
                  </p>
                </div>
                <div class="bg-white p-6 rounded-lg shadow-lg border border-green-100">
                  <div class="text-green-600 text-xl font-semibold mb-4">2세대</div>
                  <p class="text-gray-600">
                    빅데이터, AI, IoT를 활용한 정밀 생육관리 시스템
                  </p>
                </div>
                <div class="bg-white p-6 rounded-lg shadow-lg border border-green-100">
                  <div class="text-green-600 text-xl font-semibold mb-4">3세대</div>
                  <p class="text-gray-600">
                    완전 자동화된 지능형 로봇 농장 시스템
                  </p>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<style>
@keyframes blob {
  0% { transform: translate(0px, 0px) scale(1); }
  33% { transform: translate(30px, -50px) scale(1.1); }
  66% { transform: translate(-20px, 20px) scale(0.9); }
  100% { transform: translate(0px, 0px) scale(1); }
}

.animate-blob {
  animation: blob 7s infinite;
}

.animation-delay-2000 {
  animation-delay: 2s;
}

.animation-delay-4000 {
  animation-delay: 4s;
}
</style> 