<template>
  <div class="bg-white rounded-lg shadow-xl p-6">
    <h2 class="text-2xl font-bold mb-4 text-purple-800">{{ point.title }}</h2>
    <img
      :src="point.detailImage"
      :alt="point.title"
      class="w-full h-48 object-cover mb-4 rounded"
    />
    <p class="text-gray-700 mb-4">{{ point.description }}</p>
    <div class="flex justify-between">
      <button
        v-if="showCloseButton"
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
</template>

<script setup>
defineProps({
  point: {
    type: Object,
    required: true,
  },
  showCloseButton: {
    type: Boolean,
    default: true,
  },
});

defineEmits(["close", "next-point"]);
</script>
