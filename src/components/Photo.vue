<template>
  <div
    class="relative overflow-hidden will-change-transform h-screen"
    @click="$emit('image-click')"
  >
    <div
      class="w-full h-full transition-all duration-500 ease-in-out"
      :style="imageStyle"
    >
      <img
        :src="image"
        alt="Landscape with points of interest"
        class="w-full h-full object-cover"
      />
    </div>
    <div
      v-for="point in points"
      :key="point.id"
      :style="pointStyle(point)"
      class="absolute w-12 h-12 -ml-6 -mt-6 rounded-full cursor-pointer transition-all duration-200 ease-in-out will-change-transform will-change-opacity"
      :class="{
        'animate-pulse':
          !useModalLayout && !zoomedPointId && hoveredPointId !== point.id,
        'scale-150': hoveredPointId === point.id,
        hidden: !useModalLayout && zoomedPointId,
      }"
      @click.stop="$emit('point-click', point)"
      @mouseenter="hoveredPointId = point.id"
      @mouseleave="hoveredPointId = null"
    >
      <div class="w-full h-full rounded-full bg-white opacity-75"></div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from "vue";

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
  zoomLevel: {
    type: Number,
    default: 200,
  },
});

const emit = defineEmits(["point-click", "image-click"]);

const hoveredPointId = ref(null);

const activePoint = computed(() =>
  props.points.find((p) => p.id === props.zoomedPointId)
);

const imageStyle = computed(() => {
  if (props.useModalLayout || !activePoint.value) {
    return {};
  }
  const scale = props.zoomLevel / 100;
  const x = activePoint.value.x;
  const y = activePoint.value.y;
  return {
    transform: `scale(${scale})`,
    transformOrigin: `${x}% ${y}%`,
  };
});

const pointStyle = (point) => {
  const baseStyle = { left: `${point.x}%`, top: `${point.y}%` };
  if (props.useModalLayout || !activePoint.value) {
    return baseStyle;
  }
  const scale = props.zoomLevel / 100;
  const x = (point.x - activePoint.value.x) * scale + 50;
  const y = (point.y - activePoint.value.y) * scale + 50;
  return {
    ...baseStyle,
    left: `${x}%`,
    top: `${y}%`,
  };
};
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
