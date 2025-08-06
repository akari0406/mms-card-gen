<template>
  <div class="min-h-screen flex flex-col md:flex-row p-4 md:p-6 gap-6 bg-gray-50 overflow-x-hidden">
    <!-- Preview -->

    <!-- Edit Form (same as before) -->
    <div class="order-1 md:order-none w-full md:w-1/2">
      <div class="flex items-center gap-4 mb-6">
        <img src="/images/logo.png" alt="Logo" class="w-16 sm:w-20 rounded-full object-cover" />
        <div class="text-[#093E65]">
          <h2 class="text-base sm:text-2xl">បំពេញព័ត៌មានកាតមន្ត្រី</h2>
          <h2 class="text-base sm:text-2xl font-dm-serif">Officer Card Information</h2>
        </div>
      </div>

      <form
        @submit.prevent="submitForm"
        class="space-y-6 p-4 border border-gray-200 rounded-2xl shadow-md"
      >
        <div>
          <label for="name" class="block font-medium mb-1">ឈ្មោះ</label>
          <input
            id="name"
            v-model="name"
            type="text"
            class="w-full border border-gray-200 bg-white rounded-md px-4 py-1 shadow-sm focus:outline-none focus:ring-2 focus:ring-blue-900 focus:border-transparent"
            placeholder="បញ្ចូលឈ្មោះពេញ"
            required
          />
          <p class="text-sm text-gray-500 mt-1">ដាក់បានភាសាតែមួយប៉ុណ្ណោះ (ខ្មែរ ឬ អង់គ្លេស)</p>
          <p
            class="text-sm text-gray-500 mt-1"
          >Support only one language at a time (Khmer or English)</p>
        </div>

        <div>
          <label for="position" class="block font-medium mb-1">មុខតំណែង</label>
          <input
            id="position"
            v-model="position"
            type="text"
            class="w-full border border-gray-200 bg-white rounded-md px-4 py-1 shadow-sm focus:outline-none focus:ring-2 focus:ring-blue-900 focus:border-transparent"
            placeholder="បញ្ចូលមុខដំណែង"
            required
          />
                  <p
          class="text-sm text-gray-500 mt-1"
        >មន្ត្រី...</p>
        </div>

        <div>
          <label for="department" class="block font-medium mb-1">នាយកដ្ឋាន</label>
          <select
            id="department"
            v-model="department"
            class="w-full border border-gray-200 bg-white rounded-md px-4 py-1 shadow-sm focus:outline-none focus:ring-2 focus:ring-blue-900 focus:border-transparent"
            required
          >
            <option disabled value>ជ្រើសរើសអគ្គនាយកដ្ឋាន នាយកដ្ឋាន</option>
            <option>អគ្គនាយកដ្ឋាន ទស្សន៍ទាយ និងបង្ការគ្រោះ</option>
            <option>អគ្គនាយកដ្ឋាន វត្ថុទិព</option>
            <option>នាយកដ្ឋាន ទំនាក់ទំនង</option>
            <option>នាយកដ្ឋាន ប្រហាវេទមន្ត</option>
          </select>
        </div>

        <div>
          <label for="image" class="block font-medium mb-1">បង្ហោះរូបភាព</label>

          <!-- Hidden native file input -->
          <input id="image" type="file" @change="onImageChange" accept="image/*" class="hidden" />

          <!-- Custom upload button with your SVG icon -->
          <label
            for="image"
            class="inline-flex items-center gap-2 py-2 cursor-pointer border border-gray-200 bg-gray-100 shadow-sm text-gray-700 px-4 py-1 rounded-md hover:bg-gray-200 transition"
          >
            <img src="/images/image-add-line.svg" alt="Upload Icon" class="w-5 h-5" />
            Upload Image
          </label>
        </div>

        <p
          class="text-sm text-gray-500 mt-1"
        >ទិន្នន័យរបស់អ្នកមិនត្រូវបានរក្សាទុកទេ។ Your data is not stored anywhere.</p>
        <button
          type="button"
          class="w-full bg-[#093E65] text-white font-semibold py-3 rounded-md hover:bg-[#0063AC] transition flex items-center justify-center gap-2"
          onclick="document.getElementById('card-to-download').scrollIntoView({ behavior: 'smooth' });"
        >រួចរាល់</button>
      </form>
    </div>

    <div class="order-2 md:order-none w-full md:w-1/2 flex justify-center items-center my-24">
      <div
        id="card-to-download"
        class="relative aspect-[2/3] w-full max-w-sm md:w-128 bg-white rounded-lg shadow-lg flex flex-col overflow-hidden bg-cover"
        style="background-image: url('/images/watermark2.png')"
      >
        <!-- Top logo bar -->
        <div
          class="bg-gradient-to-t from-[#1970B1] to-[#093E65] text-gray-900 h-20 flex items-center justify-center"
        >
          <img src="/images/logo_long_white.png" alt="Logo" class="h-16 object-contain" />
        </div>

        <!-- Uploaded Image -->
        <div class="flex justify-center mt-8">
          <img
            v-if="imageUrl"
            :src="imageUrl"
            alt="Uploaded Image"
            class="w-24 h-32 scale-125 object-cover border-2 border-white shadow rounded"
          />
          <div
            v-else
            class="w-24 h-32 scale-125 bg-gray-300 border-2 border-white flex items-center justify-center text-gray-500 shadow rounded"
          >No Image</div>
        </div>

        <!-- Name -->
        <div class="mt-8 text-center text-white">
          <template v-if="hasKhmer(name) && hasLatin(name)">
            <div class="font-moul text-xl">{{ extractKhmer(name) }}</div>
            <div class="font-dm-serif text-lg mt-1">{{ extractLatin(name) }}</div>
          </template>
          <template v-else-if="hasKhmer(name)">
            <div class="font-moul text-xl">{{ name }}</div>
          </template>
          <template v-else>
            <div class="font-dm-serif text-xl font-semibold">{{ name || 'Full Name' }}</div>
          </template>
        </div>

        <!-- Position -->
        <div class="text-center flex mt-2 text-sm justify-center text-gray-400 mt-4 font-bold">
          <p class="me-1 font-siemreap leading-snug">កំពុងបំពេញការងារជា</p>
        </div>

        <p
          class="text-center text-2xl​ font-bold font-siemreap text-white mt-1"
        >{{ position || 'មុខតំណែង' }}</p>
        <!-- Department -->

        <p
          class="text-center text-2xl​ font-bold font-siemreap text-white mt-1"
        >នៅ{{ department || 'នាយកដ្ឋាន' }}</p>

        <!-- Effective Date -->
        <div class="text-center text-sm flex mt-auto justify-center text-gray-400 px-4 font-base">
          <p class="me-1 font-siemreap leading-snug">កាតធ្វើថ្ងៃទី</p>
          <p class="me-1 font-siemreap leading-snug">Card Issue Date</p>
        </div>
        <p class="text-center text-sm font-dm-serif text-white">{{ today }}</p>
        <!-- Department bar -->
        <div class="px-4 mt-4">
          <div
            :class="departmentBgColor(department)"
            class="mt-auto mb-4 py-2 text-center text-white font-moul text-base rounded-lg"
          >{{ department || '---' }}</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
// import html2canvas from 'html2canvas'

const name = ref("");
const position = ref("");
const department = ref("");
const workingDate = ref("");

const imageUrl = ref(null);
const today = new Date().toLocaleDateString("en-GB", {
  year: "numeric",
  month: "long",
  day: "numeric"
});

// const loading = ref(false)
// const imagePreview = ref(null)

// async function submitForm() {
//   loading.value = true

//   try {
//     const cardElement = document.querySelector('#card-to-download')
//     const canvas = await html2canvas(cardElement, {
//       scale: 2,
//       useCORS: true,
//     })

//     const image = canvas.toDataURL('image/png')
//     imagePreview.value = image

//     // Optional: auto-download
//     const link = document.createElement('a')
//     link.download = `${name.value || 'card'}.png`
//     link.href = image
//     link.click()

//   } catch (error) {
//     alert('Something went wrong while generating the image.')
//     console.error(error)
//   }

//   loading.value = false
// }

function onImageChange(event) {
  const file = event.target.files[0];
  if (!file) {
    imageUrl.value = null;
    return;
  }
  imageUrl.value = URL.createObjectURL(file);
}

// Detect Khmer characters (Unicode range U+1780–U+17FF)
function isKhmer(text) {
  if (!text) return false;
  return /[\u1780-\u17FF]/.test(text);
}

function formatDate(dateStr, locale = "km-KH") {
  const options = { year: "numeric", month: "short", day: "numeric" };
  return new Date(dateStr).toLocaleDateString(locale, options);
}

// Department background colors
function departmentBgColor(dep) {
  switch (dep) {
    case "អគ្គនាយកដ្ឋាន ទស្សន៍ទាយ និងបង្ការគ្រោះ":
      return "bg-gradient-to-r from-red-500 to-orange-300 text-white text-shadow";
    case "អគ្គនាយកដ្ឋាន វត្ថុទិព":
      return "bg-gradient-to-r from-green-400 to-indigo-500 text-white text-shadow";
    case "នាយកដ្ឋាន ទំនាក់ទំនង":
      return "bg-gradient-to-r from-sky-500 to-blue-700 text-white text-shadow";
    case "នាយកដ្ឋាន ប្រហាវេទមន្ត":
      return "bg-gradient-to-r from-violet-500 to-indigo-600 text-gray-900"; // no shadow for dark text
    default:
      return "bg-gradient-to-r from-gray-400 to-gray-600 text-white text-shadow";
  }
}

function hasKhmer(text) {
  return /[\u1780-\u17FF]/.test(text);
}

function hasLatin(text) {
  return /[a-zA-Z]/.test(text);
}
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Moul&family=Siemreap&family=DM+Serif+Text&display=swap");

.font-moul {
  font-family: "Moul", serif;
}

.font-siemreap {
  font-family: "Siemreap", sans-serif;
}
.font-dm-serif {
  font-family: "DM Serif Text", serif;
}
.text-shadow {
  text-shadow: 0 1px 2px rgba(0, 0, 0, 0.6);
}
</style>

