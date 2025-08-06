<template>
  <div class="min-h-screen flex flex-col items-center justify-center text-center bg-gray-50 px-4">
    <h1 class="text-4xl font-bold text-gray-800 mb-2">Welcome to MMS Registration</h1>
    <p class="text-lg text-gray-600 mb-6">Effortlessly manage your registration workflow</p>
    <button
      @click="goToRegister"
      class="bg-blue-600 text-white px-6 py-2 rounded-lg hover:bg-blue-700 transition"
    >
      Get Started
    </button>
  </div>
</template>

<script>
export default {
  name: 'Welcome',
  methods: {
    goToRegister() {
      this.$router.push('/register'); // Ensure this route is defined
    },
  },
};
</script>
