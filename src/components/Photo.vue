<template>
  <div
    class="relative overflow-hidden rounded-lg shadow-2xl will-change-transform"
    :class="{ 'h-[calc(100vh-8rem)]': useModalLayout }"
  >
    <div
      class="transition-all duration-200 ease-in-out h-full"
      :style="{
        transform: activePointId !== null ? 'scale(2)' : 'scale(1)',
        transformOrigin: activePoint
          ? `${activePoint.x}% ${activePoint.y}%`
          : 'center',
      }"
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
      :style="{ left: `${point.x}%`, top: `${point.y}%` }"
      class="absolute w-8 h-8 -ml-4 -mt-4 border-2 border-purple-500 rounded-full cursor-pointer transition-all duration-200 ease-in-out will-change-transform will-change-opacity hover:scale-125"
      :class="{
        'scale-125': activePointId === point.id,
        'opacity-0': activePointId !== null && activePointId !== point.id,
      }"
      @click="$emit('point-click', point)"
      @mouseenter="hoveredPointId = point.id"
      @mouseleave="hoveredPointId = null"
    >
      <div class="w-full h-full rounded-full bg-purple-500 bg-opacity-30">
        <span class="sr-only">{{ point.title }}</span>
      </div>
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
});

const emit = defineEmits(["point-click"]);

const hoveredPointId = ref(null);

const activePointId = computed(() => props.zoomedPointId);
const activePoint = computed(() =>
  props.points.find((p) => p.id === activePointId.value)
);
</script>
