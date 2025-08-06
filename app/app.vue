<template>
  <div>
    <!-- Loading screen -->
    <div
      ref="loader"
      class="fixed inset-0 z-50 flex items-center justify-center bg-[#093E65]"
    >
      <div ref="loaderContent" class="text-center text-white">
        <img
          src="/images/logo.png"
          alt="Logo"
          class="w-40 mx-auto mb-4 flicker"
        />
        <div class="text-xl font-bold py-4">ចាំតិច មេ...</div>
      </div>
    </div>

    <!-- Actual page content -->
    <div>
      <NuxtPage />
    </div>
  </div>
</template>

<script setup>
import { ref, nextTick, onMounted } from 'vue'
import { gsap } from 'gsap'
definePageMeta({
  title: 'Your Page Title', // 🔁 Change this to your desired title
})
const loader = ref(null)
const loaderContent = ref(null)


onMounted(async () => {
  await nextTick()

  requestAnimationFrame(() => {
    setTimeout(() => {
      const tl = gsap.timeline({
        onComplete: () => {
          // Fully hide the loader to prevent scroll or interaction issues
          loader.value.style.display = 'none'
        },
      })

      tl.to(loaderContent.value, {
        scale: 1.4,
        opacity: 0,
        duration: 0.6,
        ease: 'power2.inOut',
      })

      tl.to(loader.value, {
        opacity: 0,
        duration: 0.2,
        ease: 'power1.inOut',
      }, '-=0.3') // overlap fade with scale
    }, 1500)
  })
})
</script>

<style scoped>
.flicker {
  animation: flicker 1.2s infinite ease-in-out;
}

@keyframes flicker {
  0%, 100% { opacity: 1; }
  50% { opacity: 0.6; }
}
</style>
