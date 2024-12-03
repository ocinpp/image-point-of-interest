<script setup>
import { ref } from "vue";
import { Mountain, Droplet, TreePine, Flower } from "lucide-vue-next";

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

const emit = defineEmits(["select-point", "next-point", "close"]);

const toggleAccordion = (point) => {
  emit("select-point", point);
};

const getIcon = (iconName) => {
  switch (iconName) {
    case "mountain":
      return Mountain;
    case "droplet":
      return Droplet;
    case "tree-pine":
      return TreePine;
    case "flower":
      return Flower;
    default:
      return null;
  }
};

const getOptimizedImageUrl = (url) => {
  return `${url}?q=70&w=500`;
};
</script>

<template>
  <div class="p-4 pt-10 relative">
    <button
      @click="$emit('close')"
      class="absolute top-2 right-2 text-white hover:text-gray-300 focus:outline-none focus:ring-2 focus:ring-white"
      aria-label="Close accordion"
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
    <div v-for="point in points" :key="point.id" class="mb-2">
      <div
        @click="toggleAccordion(point)"
        @keydown.enter="toggleAccordion(point)"
        @keydown.space.prevent="toggleAccordion(point)"
        class="flex items-center justify-between p-2 bg-white bg-opacity-25 rounded cursor-pointer focus:outline-none focus:ring-2 focus:ring-white"
        tabindex="0"
        role="button"
        :aria-expanded="activePoint && activePoint.id === point.id"
        :aria-controls="`accordion-content-${point.id}`"
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
      <transition
        enter-active-class="transition-all duration-300 ease-out"
        enter-from-class="opacity-0 max-h-0"
        enter-to-class="opacity-100 max-h-[500px]"
        leave-active-class="transition-all duration-300 ease-in"
        leave-from-class="opacity-100 max-h-[500px]"
        leave-to-class="opacity-0 max-h-0"
      >
        <div
          v-if="activePoint && activePoint.id === point.id"
          :id="`accordion-content-${point.id}`"
          class="p-4 bg-white bg-opacity-25 rounded-b overflow-hidden"
        >
          <img
            :src="getOptimizedImageUrl(point.detailImage)"
            :alt="point.title"
            class="w-full h-48 object-cover mb-4 rounded"
          />
          <p class="text-white mb-4 description" v-html="point.description"></p>
          <div class="flex justify-center">
            <button
              @click="$emit('next-point')"
              class="text-white hover:text-gray-300 focus:outline-none focus:ring-2 focus:ring-white p-2 rounded"
              aria-label="Next point"
            >
              <svg
                xmlns="http://www.w3.org/2000/svg"
                class="h-8 w-8 animate-bounce"
                fill="none"
                viewBox="0 0 24 24"
                stroke="currentColor"
              >
                <path
                  stroke-linecap="round"
                  stroke-linejoin="round"
                  stroke-width="2"
                  d="M19 14l-7 7m0 0l-7-7m7 7V3"
                />
              </svg>
            </button>
          </div>
        </div>
      </transition>
    </div>
  </div>
</template>

<style scoped>
@keyframes bounce {
  0%,
  100% {
    transform: translateY(-25%);
    animation-timing-function: cubic-bezier(0.8, 0, 1, 1);
  }
  50% {
    transform: translateY(0);
    animation-timing-function: cubic-bezier(0, 0, 0.2, 1);
  }
}
.animate-bounce {
  animation: bounce 1s infinite;
}

.description :deep(a) {
  color: #ffffff;
  text-decoration: underline;
  transition: color 0.3s ease;
}

.description :deep(a:hover),
.description :deep(a:focus) {
  color: #f0f0f0;
  text-decoration: none;
  outline: none;
}
</style>
