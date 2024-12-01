<template>
  <div class="p-4">
    <div v-for="point in points" :key="point.id" class="mb-2">
      <div
        @click="toggleAccordion(point)"
        class="flex items-center justify-between p-2 bg-white bg-opacity-25 rounded cursor-pointer"
      >
        <div class="flex items-center">
          <component
            :is="getIcon(point.icon)"
            class="w-6 h-6 mr-2 text-white"
          />
          <span class="text-white font-semibold">{{ point.title }}</span>
        </div>
        <svg
          xmlns="http://www.w3.org/2000/svg"
          class="h-6 w-6 text-white transition-transform duration-300"
          :class="{
            'transform rotate-180': activePoint && activePoint.id === point.id,
          }"
          fill="none"
          viewBox="0 0 24 24"
          stroke="currentColor"
        >
          <path
            stroke-linecap="round"
            stroke-linejoin="round"
            stroke-width="2"
            d="M19 9l-7 7-7-7"
          />
        </svg>
      </div>
      <div
        v-if="activePoint && activePoint.id === point.id"
        class="p-4 bg-white bg-opacity-25 rounded-b"
      >
        <p class="text-white mb-4">{{ point.description }}</p>
        <button
          @click="$emit('next-point')"
          class="px-4 py-2 bg-purple-600 text-white rounded hover:bg-purple-700 transition duration-300"
        >
          Next
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
import { Mountain, Droplet, TreePine } from "lucide-vue-next";

const props = defineProps({
  points: {
    type: Array,
    required: true,
  },
  activePoint: {
    type: Object,
    default: null,
  },
});

const emit = defineEmits(["select-point", "next-point"]);

const toggleAccordion = (point) => {
  emit("select-point", point);
};

const getIcon = (iconName) => {
  switch (iconName) {
    case "mountain":
      return Mountain;
    case "droplet":
      return Droplet;
    case "tree":
      return TreePine;
    default:
      return null;
  }
};
</script>
