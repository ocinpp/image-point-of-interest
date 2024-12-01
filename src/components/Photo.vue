<template>
  <div
    class="relative overflow-hidden will-change-transform"
    :class="{ 'h-screen': !useModalLayout }"
  >
    <img
      :src="image"
      alt="Landscape with points of interest"
      class="w-full h-full object-cover"
    />
    <div
      v-for="point in points"
      :key="point.id"
      :style="{ left: `${point.x}%`, top: `${point.y}%` }"
      class="absolute w-8 h-8 -ml-4 -mt-4 rounded-full cursor-pointer transition-all duration-200 ease-in-out will-change-transform will-change-opacity hover:scale-125"
      :class="{
        'animate-pulse': !useModalLayout || activePointId !== point.id,
      }"
      @click="$emit('point-click', point)"
    >
      <div class="w-full h-full rounded-full bg-white opacity-75"></div>
    </div>
  </div>
</template>

<script setup>
import { computed } from "vue";

const props = defineProps({
  image: {
    type: String,
    required: true,
  },
  points: {
    type: Array,
    required: true,
  },
  zoomedPointId: {
    type: Number,
    default: null,
  },
  useModalLayout: {
    type: Boolean,
    default: true,
  },
});

const emit = defineEmits(["point-click"]);

const activePointId = computed(() => props.zoomedPointId);
</script>

<style scoped>
@keyframes pulse {
  0%,
  100% {
    opacity: 0.5;
    transform: scale(1);
  }
  50% {
    opacity: 1;
    transform: scale(1.1);
  }
}

.animate-pulse {
  animation: pulse 2s cubic-bezier(0.4, 0, 0.6, 1) infinite;
}
</style>
