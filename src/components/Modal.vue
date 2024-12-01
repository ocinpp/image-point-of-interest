<template>
  <div
    class="fixed inset-0 z-50 flex items-center justify-center p-4 bg-black bg-opacity-50"
  >
    <div
      class="bg-white rounded-lg shadow-xl w-full max-w-md max-h-[90vh] overflow-y-auto relative"
    >
      <button
        @click="$emit('close')"
        class="absolute top-2 right-2 text-gray-500 hover:text-gray-700"
      >
        <svg
          xmlns="http://www.w3.org/2000/svg"
          class="h-6 w-6"
          fill="none"
          viewBox="0 0 24 24"
          stroke="currentColor"
        >
          <path
            stroke-linecap="round"
            stroke-linejoin="round"
            stroke-width="2"
            d="M6 18L18 6M6 6l12 12"
          />
        </svg>
      </button>
      <div class="p-6">
        <h2 class="text-2xl font-bold mb-4 text-purple-800">
          {{ point.title }}
        </h2>
        <div class="relative h-0 pb-[56.25%] mb-4">
          <img
            :src="point.detailImage"
            :alt="point.title"
            class="absolute top-0 left-0 w-full h-full object-cover rounded"
          />
        </div>
        <p class="text-gray-700 mb-4">{{ point.description }}</p>
        <div class="flex justify-end">
          <button
            @click="$emit('next-point')"
            class="px-4 py-2 bg-purple-600 text-white rounded hover:bg-purple-700 transition duration-300"
          >
            Next
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
defineProps({
  point: {
    type: Object,
    required: true,
  },
});

defineEmits(["close", "next-point"]);
</script>
