<script setup>
import { ref, onMounted, onUnmounted, computed } from "vue";
import Home from "./components/Home.vue";
import ProjectCard from "./components/ProjectCard.vue";
import Contact from "./components/Contact.vue";

const scrollY = ref(0);
const windowHeight = ref(0);
const isTransitioning = ref(false);
const showContact = ref(false);

// Calculate scroll progress (0 to 1)
const scrollProgress = computed(() => {
  if (windowHeight.value === 0) return 0;
  return Math.min(scrollY.value / (windowHeight.value * 0.8), 1);
});

// Background color interpolation
const backgroundColor = computed(() => {
  const startColor = { r: 0, g: 0, b: 0 }; // Black
  const endColor = { r: 21, g: 21, b: 30 }; // ProjectCard background

  const r = Math.round(
    startColor.r + (endColor.r - startColor.r) * scrollProgress.value
  );
  const g = Math.round(
    startColor.g + (endColor.g - startColor.g) * scrollProgress.value
  );
  const b = Math.round(
    startColor.b + (endColor.b - startColor.b) * scrollProgress.value
  );

  return `rgb(${r}, ${g}, ${b})`;
});

// Circle scale factor based on scroll
const circleScale = computed(() => {
  return 1 - scrollProgress.value * 0.7; // Circles shrink as we scroll
});

// Home content opacity
const homeOpacity = computed(() => {
  return Math.max(0, 1 - scrollProgress.value * 2);
});

// ProjectCard opacity
const projectOpacity = computed(() => {
  return Math.max(0, (scrollProgress.value - 0.3) * 2);
});

function handleScroll() {
  scrollY.value = window.scrollY;
  isTransitioning.value =
    scrollProgress.value > 0.1 && scrollProgress.value < 0.9;
}

onMounted(() => {
  windowHeight.value = window.innerHeight;
  window.addEventListener("scroll", handleScroll);
  window.addEventListener("resize", () => {
    windowHeight.value = window.innerHeight;
  });
});

onUnmounted(() => {
  window.removeEventListener("scroll", handleScroll);
  window.removeEventListener("resize", () => {
    windowHeight.value = window.innerHeight;
  });
});
</script>

<template>
  <div class="app-container" :style="{ backgroundColor: backgroundColor }">
    <!-- Home Component with dynamic opacity -->
    <div
      class="home-section"
      :style="{
        opacity: homeOpacity,
        zIndex: scrollProgress > 0.5 ? 1 : 2,
        pointerEvents: scrollProgress > 0.5 ? 'none' : 'auto',
      }"
    >
      <Home :circle-scale="circleScale" :scroll-progress="scrollProgress" />
    </div>

    <!-- ProjectCard Component with dynamic opacity -->
    <div
      class="project-section"
      :style="{
        opacity: projectOpacity,
        zIndex: scrollProgress > 0.5 ? 2 : 1,
        pointerEvents: scrollProgress > 0.5 ? 'auto' : 'none',
      }"
    >
      <ProjectCard />
    </div>

    <button
      class="contact-fab"
      aria-label="Contact"
      @click="showContact = true"
    >
      Contact
    </button>

    <Contact
      :open="showContact"
      @close="showContact = false"
      @sent="showContact = false"
    />
  </div>
</template>

<style scoped>
.app-container {
 /* min-height: 300vh;  Allow for scrolling - increased to accommodate ProjectCard */
  transition: background-color 0.1s ease-out;
  position: relative;
}

.home-section {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100vh;
  transition: opacity 0.3s ease-out;
}

.project-section {
  position: absolute;
  top: 100vh;
  left: 0;
  width: 100%;
  transition: opacity 0.3s ease-out;
  box-sizing: border-box;
}

.contact-fab {
  position: fixed;
  right: 2rem;
  bottom: 2rem;
  z-index: 10;
  background: #5aa7ff;
  color: #0d0d1f;
  border: none;
  border-radius: 999px;
  padding: 0.6rem 1rem;
  margin-bottom: 1.8rem;
  font-family: "RalewayBold", sans-serif;
  cursor: pointer;
  box-shadow: 0 6px 20px rgba(0, 0, 0, 0.35);
}
</style>
