<script setup>
import { ref } from 'vue';
import axios from 'axios'

const userData = ref({
  email: '',
  category: ''
})

const category_options = ref([
  { text: '데이터베이스', value: 'DB'},
  { text: '네트워크', value: 'NETWORK'},
  { text: '운영체제', value: 'OS'},
  { text: '자료구조', value: 'DATA_STRUCTURE'},
  { text: '알고리즘', value: 'ALGORITHM'},
  { text: 'JAVA', value: 'JAVA'},
  { text: '스프링', value: 'SPRING'},
  { text: 'JPA', value: 'SPRING_JPA'}
])

const loading = ref(false);
const responseData = ref('');

async function subscribe() {
  loading.value = true;

  try {
    const response = await axios.post('http://localhost:8080/api/members', {
      'email': userData.value.email,
      'category': userData.value.category
    });
    responseData.value = response.data;
  } catch (error) {
    console.error(error);
    responseData.value = 'Error occurred while fetching data.';
  } finally {
    loading.value = false;
  }
}

</script>

<template>
  <div>
    <div class="w-full max-w-xl m-auto">
      <h1 class="text-4xl font-extrabold dark:text-gray-700">면접 질문 추천 서비스</h1>
      <form class="bg-white shadow-md rounded px-8 pt-6 pb-8 mb-4">
        <div class="mb-4">
          <label class="block text-gray-700 text-sm font-bold mb-2" for="username">
            E-mail
          </label>
          <input class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline" v-model="userData.email" placeholder="이메일을 입력해주세요.">
        </div>
        <div class="mb-6">
          <label class="block text-gray-700 text-sm font-bold mb-2" for="password">
            Category
          </label>
          <select v-model="userData.category" class="shadow border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline">
            <option disabled value="">카테고리를 선택해주세요.</option>
            <option v-for="category_option in category_options" :key="category_option.text" :value="category_option.value">
              {{ category_option.text }}
            </option>
          </select>
        </div>
        <div class="flex items-center justify-between">
          <button class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded focus:outline-none focus:shadow-outline" type="button" @click="subscribe">
            구독하기
          </button>
        </div>
      </form>
      <div v-if="loading" role="status" class="text-center">
        <svg aria-hidden="true" class="w-8 h-8 mr-2 text-gray-200 animate-spin dark:text-gray-600 fill-blue-600" viewBox="0 0 100 101" fill="none" xmlns="http://www.w3.org/2000/svg">
          <path d="M100 50.5908C100 78.2051 77.6142 100.591 50 100.591C22.3858 100.591 0 78.2051 0 50.5908C0 22.9766 22.3858 0.59082 50 0.59082C77.6142 0.59082 100 22.9766 100 50.5908ZM9.08144 50.5908C9.08144 73.1895 27.4013 91.5094 50 91.5094C72.5987 91.5094 90.9186 73.1895 90.9186 50.5908C90.9186 27.9921 72.5987 9.67226 50 9.67226C27.4013 9.67226 9.08144 27.9921 9.08144 50.5908Z" fill="currentColor"/>
          <path d="M93.9676 39.0409C96.393 38.4038 97.8624 35.9116 97.0079 33.5539C95.2932 28.8227 92.871 24.3692 89.8167 20.348C85.8452 15.1192 80.8826 10.7238 75.2124 7.41289C69.5422 4.10194 63.2754 1.94025 56.7698 1.05124C51.7666 0.367541 46.6976 0.446843 41.7345 1.27873C39.2613 1.69328 37.813 4.19778 38.4501 6.62326C39.0873 9.04874 41.5694 10.4717 44.0505 10.1071C47.8511 9.54855 51.7191 9.52689 55.5402 10.0491C60.8642 10.7766 65.9928 12.5457 70.6331 15.2552C75.2735 17.9648 79.3347 21.5619 82.5849 25.841C84.9175 28.9121 86.7997 32.2913 88.1811 35.8758C89.083 38.2158 91.5421 39.6781 93.9676 39.0409Z" fill="currentFill"/>
        </svg>
        <span class="sr-only">Loading...</span>
      </div>
      <div v-else>{{ responseData }}</div>
    </div>
  </div>
</template>