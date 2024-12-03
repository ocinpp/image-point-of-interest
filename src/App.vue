<template>
  <div
    class="min-h-screen bg-gradient-to-br from-purple-100 to-indigo-200 font-sans"
  >
    <div class="relative">
      <Photo
        :image="image"
        :points="points"
        :zoomed-point-id="zoomedPointId"
        :use-modal-layout="useModalLayout"
        :zoom-level="zoomLevel"
        @point-click="handlePointClick"
        @image-click="handleImageClick"
      />
      <h1
        v-if="!useModalLayout"
        class="absolute top-4 left-4 text-4xl font-bold text-white shadow-text"
      >
        Discover Nature's Wonders
      </h1>
      <Transition name="slide">
        <div
          v-if="!useModalLayout && showAccordion"
          class="absolute top-0 right-0 w-3/10 h-full bg-black bg-opacity-50 overflow-y-auto"
        >
          <Accordion
            :points="points"
            :active-point="selectedPoint"
            @select-point="handlePointClick"
            @next-point="handleNextPoint"
            @close="closeAccordion"
          />
        </div>
      </Transition>
    </div>
    <Modal
      v-if="useModalLayout && selectedPoint"
      :point="selectedPoint"
      @close="handleCloseModal"
      @next-point="handleNextPoint"
    />
  </div>
</template>

<script setup>
import { ref } from "vue";
import Photo from "./components/Photo.vue";
import Modal from "./components/Modal.vue";
import Accordion from "./components/Accordion.vue";

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
    icon: "mountain",
  },
  {
    id: 2,
    x: 60,
    y: 50,
    title: "Serene Lake",
    description:
      "A calm lake reflecting the surrounding landscape, offering a peaceful retreat.",
    detailImage: "https://images.unsplash.com/photo-1503614472-8c93d56e92ce",
    icon: "droplet",
  },
  {
    id: 3,
    x: 80,
    y: 70,
    title: "Lush Forest",
    description:
      "A vibrant green forest teeming with life, representing the Earth's biodiversity.",
    detailImage: "https://images.unsplash.com/photo-1448375240586-882707db888b",
    icon: "tree-pine",
  },
];

const selectedPoint = ref(null);
const zoomedPointId = ref(null);
const useModalLayout = ref(false);
const showAccordion = ref(false);
const zoomLevel = ref(400);

const handlePointClick = (point) => {
  selectedPoint.value = point;
  zoomedPointId.value = point.id;
  showAccordion.value = true;
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

const closeAccordion = () => {
  showAccordion.value = false;
  selectedPoint.value = null;
  zoomedPointId.value = null;
};

const handleImageClick = () => {
  if (showAccordion.value) {
    closeAccordion();
  }
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

.shadow-text {
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5);
}

.will-change-transform {
  will-change: transform;
}

.will-change-opacity {
  will-change: opacity;
}

.slide-enter-active,
.slide-leave-active {
  transition: transform 0.3s ease-in-out;
}

.slide-enter-from,
.slide-leave-to {
  transform: translateX(100%);
}
</style>
