<script setup>
import { ref, onMounted, computed } from "vue";

const props = defineProps({
  circleScale: {
    type: Number,
    default: 1
  },
  scrollProgress: {
    type: Number,
    default: 0
  }
});

const titleRef = ref(null);
const subtitleRef = ref(null);
const navSvgRef = ref(null);
const navSpanRef = ref(null);

// Dynamic circle styles based on scroll
const circleStyles = computed(() => {
  return {
    transform: `scale(${props.circleScale})`,
    opacity: Math.max(0, 1 - props.scrollProgress * 1.5)
  };
});

onMounted(() => {
  setTimeout(() => {
    if (titleRef.value) titleRef.value.style.opacity = "1";
    if (subtitleRef.value) subtitleRef.value.style.opacity = "1";
    if (navSvgRef.value) navSvgRef.value.style.opacity = "1";
    if (navSpanRef.value) navSpanRef.value.style.opacity = "1";
  }, 600);
});

</script>

<template>
  <main class="centered-content">
    <header>
      <h1 ref="titleRef" style="transition: opacity 0.3s ease;">This is my portfolio</h1>
      <p ref="subtitleRef" style="transition: opacity 0.3s ease;">Welcome on board</p>
    </header>
    <div class="circles" :style="circleStyles">
      <ul>
        <li class="circle"></li>
        <li class="circle"></li>
        <li class="circle"></li>
      </ul>
    </div>
    <nav aria-label="Scroll down">
      <svg
        ref="navSvgRef"
        xmlns="http://www.w3.org/2000/svg"
        width="24"
        height="24"
        viewBox="0 0 24 24"
        fill="none"
        stroke="currentColor"
        stroke-width="2"
        stroke-linecap="round"
        stroke-linejoin="round"
        class="lucide lucide-triangle-icon lucide-triangle"
        style="transform: rotate(180deg)"
      >
        <path
          d="M13.73 4a2 2 0 0 0-3.46 0l-8 14A2 2 0 0 0 4 21h16a2 2 0 0 0 1.73-3Z"
        />
      </svg>
      <span ref="navSpanRef" class="scroll-hint">Scroll down to have a look</span>
    </nav>
  </main>
</template>

<style scoped>
@font-face {
  font-family: "RalewayBold";
  src: url("/fonts/Raleway-Bold.ttf") format("truetype");
  font-weight: bold;
  font-style: normal;
}
@font-face {
  font-family: "RalewayExtraBold";
  src: url("/fonts/Raleway-ExtraBold.ttf") format("truetype");
  font-weight: 800;
  font-style: normal;
}
@font-face {
  font-family: "RalewaySemiBold";
  src: url("/fonts/Raleway-SemiBold.ttf") format("truetype");
  font-weight: 600;
  font-style: normal;
}
@font-face {
  font-family: "RalewayMedium";
  src: url("/fonts/Raleway-Medium.ttf") format("truetype");
  font-weight: 500;
  font-style: normal;
}

/* Center the main content vertically and horizontally */
.centered-content {
  min-height: 100vh; /* fill viewport height */
  display: flex;
  flex-direction: column;
  justify-content: center; /* vertical centering */
  align-items: center; /* horizontal centering */
  box-sizing: border-box;
  padding: 2rem 1rem;
}

header {
  text-align: center;
}

header h1 {
  font-family: "RalewayExtraBold", sans-serif;
  font-size: 28px;
  color: #fff;
  opacity: 0;
}

header p {
  font-family: "RalewaySemiBold", sans-serif;
  font-size: 28px;
  color: #fff;
  opacity: 0;
}

/* ensure background circles are contained and behind content */
.centered-content {
  position: relative;
  overflow: hidden;
}

/* background circles wrapper */
.circles {
  position: absolute;
  inset: 0;
  pointer-events: none;
  z-index: -1;
  transition: transform 0.1s ease-out, opacity 0.3s ease-out;
}

.circles ul {
  position: absolute;
  inset: 0;
  margin: 0;
  padding: 0;
  list-style: none;
}

/* base style for each circle */
.circles .circle {
  position: absolute;
  border-radius: 50%;
  transform: translate(-50%, -50%);
  filter: blur(20px);
  opacity: 1;
  mix-blend-mode: screen;
  will-change: transform, opacity;
  /* soft radial fade for nicer look */
  background: radial-gradient(
    circle at 30% 30%,
    rgba(255, 255, 255, 0.35),
    rgba(255, 255, 255, 0.05)
  );
  transition: opacity 0.4s linear;
  animation: floatScale 6s ease-in-out infinite;
}

/* give each circle a different (pseudo-random) size & position & color */
.circles .circle:nth-child(1) {
  --size: 260px;
  width: var(--size);
  height: var(--size);
  left: 12%;
  top: 18%;
  background: radial-gradient(
    circle at 30% 30%,
    rgba(255, 140, 200, 0.4),
    rgba(255, 60, 160, 0.08)
  );
  animation-duration: 7s;
}
.circles .circle:nth-child(2) {
  --size: 420px;
  width: var(--size);
  height: var(--size);
  left: 82%;
  top: 40%;
  background: radial-gradient(
    circle at 30% 30%,
    rgba(100, 200, 255, 0.35),
    rgba(40, 120, 255, 0.08)
  );
  animation-duration: 8s;
}
.circles .circle:nth-child(3) {
  --size: 140px;
  width: var(--size);
  height: var(--size);
  left: 60%;
  top: 85%;
  background: radial-gradient(
    circle at 30% 30%,
    rgba(120, 255, 200, 0.3),
    rgba(10, 180, 120, 0.06)
  );
  animation-duration: 5s;
}

/* more noticeable pulsing/scale to simulate organic movement */
@keyframes floatScale {
  0% {
    transform: translate(-50%, -50%) scale(1) rotate(0deg);
  }
  25% {
    transform: translate(-50%, -50%) scale(1.15) rotate(90deg);
  }
  50% {
    transform: translate(-50%, -50%) scale(1.25) rotate(180deg);
  }
  75% {
    transform: translate(-50%, -50%) scale(1.15) rotate(270deg);
  }
  100% {
    transform: translate(-50%, -50%) scale(1) rotate(360deg);
  }
}

/* respect reduced motion preference */
@media (prefers-reduced-motion: reduce) {
  .circles .circle {
    animation: none;
  }
}

/* make nav layout predictable when centering */
nav {
  text-align: center;
  position: relative;
  display: flex;
  align-items: center;
  margin-top: 1rem;
}

nav svg {
  color: #fff;
  margin-right: 16px;
  opacity: 0;
}

nav span {
  font-family: "RalewaySemiBold", sans-serif;
  font-size: 0.7rem;
  color: #fff;
  position: static; /* remove absolute positioning so it participates in flex layout */
  opacity: 0;
}
</style>