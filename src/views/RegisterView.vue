<script setup>
import { ref } from 'vue'
import { useRouter } from 'vue-router'

const router = useRouter()

const form = ref({
  userId: '',
  password: '',
  passwordConfirm: '',
  address: '',
  addressDetail: '',
  zonecode: ''
})

const errors = ref({
  userId: '',
  password: '',
  passwordConfirm: ''
})

const validateForm = () => {
  let isValid = true
  errors.value = {
    userId: '',
    password: '',
    passwordConfirm: ''
  }

  if (!form.value.userId) {
    errors.value.userId = '아이디를 입력해주세요.'
    isValid = false
  }

  if (!form.value.password) {
    errors.value.password = '비밀번호를 입력해주세요.'
    isValid = false
  } else if (form.value.password.length < 6) {
    errors.value.password = '비밀번호는 6자 이상이어야 합니다.'
    isValid = false
  }

  if (form.value.password !== form.value.passwordConfirm) {
    errors.value.passwordConfirm = '비밀번호가 일치하지 않습니다.'
    isValid = false
  }

  return isValid
}

const handleSubmit = () => {
  if (validateForm()) {
    // TODO: API 호출
    console.log('Form submitted:', form.value)
    router.push('/login')
  }
}

const openAddressSearch = () => {
  new window.daum.Postcode({
    oncomplete: (data) => {
      form.value.zonecode = data.zonecode
      form.value.address = data.address
    }
  }).open()
}
</script>

<template>
  <div class="min-h-screen bg-gray-50 flex flex-col justify-center py-12 sm:px-6 lg:px-8 relative overflow-hidden">
    <!-- 배경 요소 -->
    <div class="absolute inset-0 z-0">
      <div class="absolute inset-0 bg-gradient-to-br from-green-100 to-gray-200"></div>
      <div class="absolute top-0 left-0 right-0 h-96 bg-gradient-to-b from-green-200/70"></div>
      <div class="absolute inset-0 bg-[url('https://images.unsplash.com/photo-1530836369250-ef72a3f5cda8')] opacity-30 bg-cover bg-center mix-blend-multiply"></div>
      <div class="absolute inset-y-0 right-0 w-1/2 bg-gradient-to-l from-green-100/90"></div>
      <div class="absolute inset-y-0 left-0 w-1/2 bg-gradient-to-r from-green-100/90"></div>
    </div>

    <div class="sm:mx-auto sm:w-full sm:max-w-md relative z-10">
      <h2 class="mt-6 text-center text-3xl font-bold tracking-tight text-gray-900">
        회원가입
      </h2>
    </div>

    <div class="mt-8 sm:mx-auto sm:w-full sm:max-w-md relative z-10">
      <div class="bg-white/80 backdrop-blur-sm py-8 px-4 shadow-xl sm:rounded-lg sm:px-10 border border-gray-100">
        <form class="space-y-6" @submit.prevent="handleSubmit">
          <div>
            <label for="userId" class="block text-sm font-medium text-gray-700">아이디</label>
            <div class="mt-1">
              <input
                id="userId"
                v-model="form.userId"
                type="text"
                required
                class="block w-full appearance-none rounded-md border border-gray-300 px-3 py-2 placeholder-gray-400 shadow-sm focus:border-green-500 focus:outline-none focus:ring-green-500"
              />
              <p v-if="errors.userId" class="mt-1 text-sm text-red-600">{{ errors.userId }}</p>
            </div>
          </div>

          <div>
            <label for="password" class="block text-sm font-medium text-gray-700">비밀번호</label>
            <div class="mt-1">
              <input
                id="password"
                v-model="form.password"
                type="password"
                required
                class="block w-full appearance-none rounded-md border border-gray-300 px-3 py-2 placeholder-gray-400 shadow-sm focus:border-green-500 focus:outline-none focus:ring-green-500"
              />
              <p v-if="errors.password" class="mt-1 text-sm text-red-600">{{ errors.password }}</p>
            </div>
          </div>

          <div>
            <label for="passwordConfirm" class="block text-sm font-medium text-gray-700">비밀번호 확인</label>
            <div class="mt-1">
              <input
                id="passwordConfirm"
                v-model="form.passwordConfirm"
                type="password"
                required
                class="block w-full appearance-none rounded-md border border-gray-300 px-3 py-2 placeholder-gray-400 shadow-sm focus:border-green-500 focus:outline-none focus:ring-green-500"
              />
              <p v-if="errors.passwordConfirm" class="mt-1 text-sm text-red-600">{{ errors.passwordConfirm }}</p>
            </div>
          </div>

          <div>
            <label for="address" class="block text-sm font-medium text-gray-700">주소</label>
            <div class="mt-1 space-y-2">
              <div class="flex gap-2">
                <input
                  id="zonecode"
                  v-model="form.zonecode"
                  type="text"
                  readonly
                  placeholder="우편번호"
                  class="block w-32 appearance-none rounded-md border border-gray-300 px-3 py-2 placeholder-gray-400 shadow-sm bg-gray-50"
                />
                <button
                  type="button"
                  @click="openAddressSearch"
                  class="inline-flex justify-center rounded-md border border-transparent bg-green-600 py-2 px-4 text-sm font-medium text-white shadow-sm hover:bg-green-700 focus:outline-none focus:ring-2 focus:ring-green-500 focus:ring-offset-2"
                >
                  주소 검색
                </button>
              </div>
              <input
                id="address"
                v-model="form.address"
                type="text"
                readonly
                placeholder="주소"
                class="block w-full appearance-none rounded-md border border-gray-300 px-3 py-2 placeholder-gray-400 shadow-sm bg-gray-50"
              />
              <input
                id="addressDetail"
                v-model="form.addressDetail"
                type="text"
                placeholder="상세주소"
                class="block w-full appearance-none rounded-md border border-gray-300 px-3 py-2 placeholder-gray-400 shadow-sm focus:border-green-500 focus:outline-none focus:ring-green-500"
              />
            </div>
          </div>

          <div>
            <button
              type="submit"
              @click="handleSubmit"
              class="flex w-full justify-center rounded-md border border-transparent bg-green-600 py-2 px-4 text-sm font-medium text-white shadow-sm hover:bg-green-700 focus:outline-none focus:ring-2 focus:ring-green-500 focus:ring-offset-2"
            >
              가입하기
            </button>
          </div>
        </form>

        <div class="mt-6 text-center text-sm">
          <span class="text-gray-600">이미 계정이 있으신가요?</span>
          <a href="/login" class="ml-1 font-medium text-green-600 hover:text-green-500">
            로그인
          </a>
        </div>
      </div>
    </div>
  </div>
</template> 