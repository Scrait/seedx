<template>
  <div class="flex items-center justify-center min-h-screen bg-gradient-to-tr from-[#101010] to-[#192630]">
    <div class="bg-white p-8 rounded-2xl shadow-lg w-full max-w-sm not-draggable">
      <h1 class="text-3xl font-bold text-center text-gray-900 mb-6">Авторизация</h1>
      <form>
        <div class="mb-4">
          <label for="username" class="block text-gray-700">Логин</label>
          <input
            id="username"
            type="text"
            class="w-full px-4 py-2 border-2 border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-indigo-500"
            placeholder="Введите логин"
          />
        </div>
        <div class="mb-4">
          <label for="password" class="block text-gray-700">Пароль</label>
          <input
            id="password"
            type="password"
            class="w-full px-4 py-2 border-2 border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-indigo-500"
            placeholder="Введите пароль"
          />
        </div>
        <button
          type="button"
          @click="goToSoftware"
          class="w-full bg-gradient-to-r from-pink-500 via-orange-500 to-yellow-500 text-white font-bold px-4 py-2 rounded-lg transition duration-300 hover:bg-gradient-to-l"
        >
          Войти
        </button>
      </form>

      <div class="mt-4 text-center">
        <p class="text-gray-600">
          Нет аккаунта?
          <router-link to="/register" class="text-blue-600 hover:text-blue-500 font-semibold">Зарегистрироваться</router-link>
        </p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  methods: {
    goToSoftware() {
      this.$router.push('/software'); // Переход на страницу софта
    },
  },
};
</script>

<style scoped>
/* Дополнительные стили, если нужно */
</style>
