<script setup>
import { ref, onMounted } from 'vue'
import { useRouter } from 'vue-router'

const router = useRouter()
const email = ref('')
const password = ref('')

// 카카오 초기화
onMounted(() => {
  if (window.Kakao && !window.Kakao.isInitialized()) {
    // 여기에 JavaScript 키를 넣어주세요
    window.Kakao.init('93457dbb4b26be20389dd351b945afd3')
  }

  // Google 로그인 초기화
  window.google?.accounts.id.initialize({
    // 여기에 OAuth 2.0 클라이언트 ID를 넣어주세요
    client_id: '1074215545405-jh2tn55dva1a4fkgg47iip3e2v6l4d0r.apps.googleusercontent.com',
    callback: handleGoogleLogin
  })
})

const handleLogin = () => {
  if (email.value === 'jhkim@nate.com' && password.value === 'jhkim') {
    router.push('/main')
  } else {
    alert('아이디 또는 비밀번호가 올바르지 않습니다.')
  }
}

const handleKakaoLogin = () => {
  window.Kakao.Auth.login({
    success: (authObj) => {
      console.log('카카오 로그인 성공', authObj)
      // 사용자 정보 가져오기
      window.Kakao.API.request({
        url: '/v2/user/me',
        success: (res) => {
          console.log('카카오 사용자 정보', res)
          // 여기서 백엔드로 사용자 정보를 전송하고 처리할 수 있습니다
          router.push('/main')
        },
        fail: (error) => {
          console.error('사용자 정보 요청 실패', error)
          alert('카카오 로그인 중 오류가 발생했습니다.')
        }
      })
    },
    fail: (err) => {
      console.error('카카오 로그인 실패', err)
      alert('카카오 로그인 중 오류가 발생했습니다.')
    }
  })
}

const handleGoogleLogin = (response) => {
  // JWT 토큰을 포함한 응답을 받습니다
  const credential = response.credential
  console.log('Google 로그인 성공:', credential)
  
  // 여기서 백엔드로 credential을 전송하고 처리할 수 있습니다
  router.push('/main')
}

const handleGoogleLoginClick = () => {
  window.google?.accounts.id.prompt()
}

const goToHome = () => {
  router.push('/')
}
</script>

<template>
  <div class="min-h-screen bg-gray-50 flex flex-col justify-center py-12 sm:px-6 lg:px-8 relative overflow-hidden">
    <!-- 배경 요소 -->
    <div class="absolute inset-0 z-0">
      <div class="absolute inset-0 bg-gradient-to-br from-green-50 to-gray-50"></div>
      <div class="absolute top-0 left-0 right-0 h-96 bg-gradient-to-b from-green-200/70"></div>
      <div class="absolute inset-0 bg-[url('https://images.unsplash.com/photo-1530836369250-ef72a3f5cda8')] opacity-100 bg-cover bg-center mix-blend-multiply"></div>
      <div class="absolute inset-y-0 right-0 w-1/2 bg-gradient-to-l from-green-100/90"></div>
      <div class="absolute inset-y-0 left-0 w-1/2 bg-gradient-to-r from-green-100/90"></div>
    </div>

    <!-- 메인 컨텐츠 -->
    <div class="relative z-10">
      <div class="sm:mx-auto sm:w-full sm:max-w-md">
        <h2 class="text-center text-4xl font-bold tracking-tight text-gray-900">
          스마트팜
        </h2>
        <p class="mt-2 text-center text-lg text-gray-600 font-medium">
          수직농장 관리 시스템
        </p>
      </div>

      <div class="mt-8 sm:mx-auto sm:w-full sm:max-w-md">
        <div class="bg-white/80 backdrop-blur-sm py-8 px-4 shadow-xl sm:rounded-lg sm:px-10 border border-gray-100">
          <form class="space-y-6" @submit.prevent="handleLogin">
            <div>
              <label for="email" class="block text-sm font-medium text-gray-700">아이디</label>
              <div class="mt-1">
                <input
                  id="email"
                  v-model="email"
                  type="text"
                  required
                  class="block w-full appearance-none rounded-md border border-gray-300 px-3 py-2 placeholder-gray-400 shadow-sm focus:border-green-500 focus:outline-none focus:ring-green-500"
                />
              </div>
            </div>

            <div>
              <label for="password" class="block text-sm font-medium text-gray-700">비밀번호</label>
              <div class="mt-1">
                <input
                  id="password"
                  v-model="password"
                  type="password"
                  required
                  class="block w-full appearance-none rounded-md border border-gray-300 px-3 py-2 placeholder-gray-400 shadow-sm focus:border-green-500 focus:outline-none focus:ring-green-500"
                />
              </div>
            </div>

            <div class="flex items-center justify-between">
              <div class="flex items-center">
                <input
                  id="remember-me"
                  type="checkbox"
                  class="h-4 w-4 rounded border-gray-300 text-green-600 focus:ring-green-500"
                />
                <label for="remember-me" class="ml-2 block text-sm text-gray-900">로그인 상태 유지</label>
              </div>

              <div class="text-sm">
                <a href="#" class="font-medium text-green-600 hover:text-green-500">
                  비밀번호를 잊으셨나요?
                </a>
              </div>
            </div>

            <div>
              <button
                type="submit"
                class="flex w-full justify-center rounded-md border border-transparent bg-green-600 py-2 px-4 text-sm font-medium text-white shadow-sm hover:bg-green-700 focus:outline-none focus:ring-2 focus:ring-green-500 focus:ring-offset-2"
              >
                로그인
              </button>
            </div>
          </form>

          <div class="mt-6">
            <div class="relative">
              <div class="absolute inset-0 flex items-center">
                <div class="w-full border-t border-gray-300"></div>
              </div>
              <div class="relative flex justify-center text-sm">
                <!-- <span class="bg-white/80 px-2 text-gray-500">또는 다음으로 계속하기</span> -->
              </div>
            </div>

            <div class="mt-6 grid grid-cols-2 gap-3">
              <div>
                <button
                  type="button"
                  @click="handleGoogleLoginClick"
                  class="inline-flex w-full justify-center rounded-md border border-gray-300 bg-white py-2 px-4 text-sm font-medium text-gray-500 shadow-sm hover:bg-gray-50"
                >
                  <span class="sr-only">Google로 로그인</span>
                  <svg class="h-5 w-5" viewBox="0 0 24 24">
                    <path
                      fill="currentColor"
                      d="M12.48 10.92v3.28h7.84c-.24 1.84-.853 3.187-1.787 4.133-1.147 1.147-2.933 2.4-6.053 2.4-4.827 0-8.6-3.893-8.6-8.72s3.773-8.72 8.6-8.72c2.6 0 4.507 1.027 5.907 2.347l2.307-2.307C18.747 1.44 16.133 0 12.48 0 5.867 0 .307 5.387.307 12s5.56 12 12.173 12c3.573 0 6.267-1.173 8.373-3.36 2.16-2.16 2.84-5.213 2.84-7.667 0-.76-.053-1.467-.173-2.053H12.48z"
                    />
                  </svg>
                </button>
              </div>

              <div>
                <button
                  type="button"
                  @click="handleKakaoLogin"
                  class="inline-flex w-full justify-center rounded-md border border-gray-300 bg-[#FEE500] py-2 px-4 text-sm font-medium text-gray-900 shadow-sm hover:bg-[#FEE500]/90"
                >
                  <span class="sr-only">Kakao로 로그인</span>
                  <svg class="h-5 w-5" viewBox="0 0 24 24">
                    <path
                      fill="currentColor"
                      d="M12 3c5.799 0 10.5 3.664 10.5 8.185 0 4.52-4.701 8.184-10.5 8.184a13.5 13.5 0 0 1-1.727-.11l-4.408 2.883c-.501.265-.974.06-.974-.422V18.5c-2.137-1.382-3.39-3.468-3.39-5.814C1.5 6.664 6.201 3 12 3Z"
                    />
                  </svg>
                </button>
              </div>
            </div>
          </div>

          <div class="mt-6 text-center text-sm">
            <span class="text-gray-600">아직 계정이 없으신가요?</span>
            <router-link to="/register" class="ml-1 font-medium text-green-600 hover:text-green-500">
              회원가입
            </router-link>
          </div>
        </div>
      </div>
    </div>
  </div>
</template> 