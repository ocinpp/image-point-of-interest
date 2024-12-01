<template>
  <div class="fixed inset-0 z-50 flex items-center justify-center p-4 bg-black bg-opacity-50">
    <div class="bg-white rounded-lg shadow-xl w-full max-w-4xl max-h-[90vh] overflow-y-auto">
      <div class="p-6">
        <h2 class="text-2xl font-bold mb-4 text-purple-800">{{ point.title }}</h2>
        <div class="relative h-0 pb-[56.25%] mb-4">
          <img
            :src="point.detailImage"
            :alt="point.title"
            class="absolute top-0 left-0 w-full h-full object-cover rounded"
          />
        </div>
        <p class="text-gray-700 mb-4">{{ point.description }}</p>
        <div class="flex justify-between">
          <button
            @click="$emit('close')"
            class="px-4 py-2 bg-purple-600 text-white rounded hover:bg-purple-700 transition duration-300"
          >
            Close
          </button>
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
    required: true
  }
});

defineEmits(['close', 'next-point']);
</script>