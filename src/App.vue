<template>
  <div
    class="min-h-screen bg-gradient-to-br from-purple-100 to-indigo-200 p-8 font-sans"
  >
    <div class="max-w-7xl mx-auto">
      <h1 class="text-4xl font-bold text-center mb-8 text-purple-800">
        Discover Nature's Wonders
      </h1>
      <div :class="{ flex: !useModalLayout }">
        <div :class="{ 'w-7/10': !useModalLayout, 'w-full': useModalLayout }">
          <Photo
            :image="image"
            :points="points"
            :zoomed-point-id="zoomedPointId"
            :use-modal-layout="useModalLayout"
            @point-click="handlePointClick"
          />
        </div>
        <div v-if="!useModalLayout" class="w-3/10 pl-4">
          <PointInfo
            v-if="selectedPoint"
            :point="selectedPoint"
            :show-close-button="false"
          />
        </div>
      </div>
      <Modal
        v-if="useModalLayout && selectedPoint"
        :point="selectedPoint"
        @close="handleCloseModal"
        @next-point="handleNextPoint"
      />
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
import Photo from "./components/Photo.vue";
import Modal from "./components/Modal.vue";
import PointInfo from "./components/PointInfo.vue";

const image = "https://images.unsplash.com/photo-1506744038136-46273834b3fb";

const points = [
  {
    id: 1,
    x: 20,
    y: 30,
    title: "Mountain Peak",
    description:
      "A majestic mountain peak rising above the clouds, showcasing nature's grandeur.",
    detailImage: "https://images.unsplash.com/photo-1464822759023-fed622ff2c3b",
  },
  {
    id: 2,
    x: 60,
    y: 50,
    title: "Serene Lake",
    description:
      "A calm lake reflecting the surrounding landscape, offering a peaceful retreat.",
    detailImage: "https://images.unsplash.com/photo-1503614472-8c93d56e92ce",
  },
  {
    id: 3,
    x: 80,
    y: 70,
    title: "Lush Forest",
    description:
      "A vibrant green forest teeming with life, representing the Earth's biodiversity.",
    detailImage: "https://images.unsplash.com/photo-1448375240586-882707db888b",
  },
];

const selectedPoint = ref(null);
const zoomedPointId = ref(null);
const useModalLayout = ref(true); // Control layout here: true for modal, false for side-by-side

const handlePointClick = (point) => {
  selectedPoint.value = point;
  zoomedPointId.value = point.id;
};

const handleCloseModal = () => {
  selectedPoint.value = null;
  zoomedPointId.value = null;
};

const handleNextPoint = () => {
  const currentIndex = points.findIndex((p) => p.id === zoomedPointId.value);
  const nextIndex = (currentIndex + 1) % points.length;
  handlePointClick(points[nextIndex]);
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600;700&display=swap");

.font-sans {
  font-family: "Poppins", sans-serif;
}

.w-7\/10 {
  width: 70%;
}
.w-3\/10 {
  width: 30%;
}

.will-change-transform {
  will-change: transform;
}

.will-change-opacity {
  will-change: opacity;
}
</style>
