<script setup>
import { ref, computed, onMounted, onUnmounted } from "vue";

const projects = [
  {
    title: "F1 project",
    image: "public/img/F1-screenshot.webp",
    secImage: "public/img/newImg/F1.png",
    desc: "A front-end OpenF1 API based project.",
    video: "public/video/F1ScreenRec.mp4",
    stack: [
      { name: "CSS3", icon: ["fab", "css3-alt"] },
      { name: "JavaScript", icon: ["fab", "js-square"] },
      { name: "HTML5", icon: ["fab", "html5"] },
    ],
    color: "#FFFFFF", // font color
    bgColor: "#15151E", // background color
    iColor: "#FF1800",
    uColor: "#FF1800",
  },
  {
    title: "Réflexologie Côtière",
    image: "public/img/Reflexo-screenshot.webp",
    secImage: "public/img/newImg/RC.png",
    desc: "A Symfony webapp meant for a reflexologist.",
    video: "public/video/RefleCoScreenRec.mp4",
    stack: [
      { name: "CSS3", icon: ["fab", "css3-alt"] },
      { name: "Twig", icon: ["fab", "symfony"] },
      { name: "JavaScript", icon: ["fab", "js-square"] },
      { name: "Symfony", icon: ["fab", "symfony"] },
      { name: "MySQL", icon: ["fas", "database"] },
    ],
    color: "#47301E",
    bgColor: "#D2CAC2",
    iColor: "#849779",
    uColor: "#849779",
    gitHub: "https://github.com/SamyMechiche/reflexologieCotiere",
  },
  {
    title: "Nihon",
    image: "public/img/Nihon-screenshot.webp",
    desc: "A dynamic online manga store.",
    video: "public/video/RefleCoScreenRec.mp4",
    stack: [
      { name: "CSS3", icon: ["fab", "css3-alt"] },
      { name: "HTML5", icon: ["fab", "html5"] },
      { name: "PHP", icon: ["fab", "php"] },
      { name: "JavaScript", icon: ["fab", "js-square"] },
      { name: "MySQL", icon: ["fas", "database"] },
    ],
    color: "#4A8BA0",
    bgColor: "#201F24",
    iColor: "#CFCFCF",
    uColor: "#CFCFCF",
    gitHub: "https://github.com/Jackmaa/nihon-ECF-3",
  },
  {
    title: "Korea trip budgeting app",
    image: "public/img/Korea-screenshot.png",
    desc: "A dynamic budgeting webapp for a trip to Korea",
    video: "public/video/KFScreenRec.mp4",
    stack: [
      { name: "HTML5", icon: ["fab", "html5"] },
      { name: "CSS3", icon: ["fab", "css3-alt"] },
      { name: "JavaScript", icon: ["fab", "js-square"] },
    ],
    color: "#488FF7",
    bgColor: "#0D0D1F",
    iColor: "#CFCFCF",
    uColor: "#CFCFCF",
    gitHub: "https://github.com/SamyMechiche/korea-funding-app",
  },
];

const currentIndex = ref(0);
const project = computed(() => projects[currentIndex.value]);
const windowWidth = ref(0);

// Check if we should use desktop layout
const isDesktop = computed(() => windowWidth.value >= 900);

// Transition state management
const isTransitioning = ref(false);
const isImageTransitioning = ref(false);
const transitionDuration = 800; // milliseconds
const imageTransitionDuration = 600; // milliseconds

function updateWindowWidth() {
  windowWidth.value = window.innerWidth;
}

onMounted(() => {
  updateWindowWidth();
  window.addEventListener("resize", updateWindowWidth);
});

onUnmounted(() => {
  window.removeEventListener("resize", updateWindowWidth);
});

function nextProject() {
  if (isTransitioning.value) return;
  console.log("clicked");
  isTransitioning.value = true;
  isImageTransitioning.value = true;

  // Start image fade out
  setTimeout(() => {
    // Change project (this triggers image change)
    currentIndex.value = (currentIndex.value + 1) % projects.length;
    // Start image fade in
    setTimeout(() => {
      isImageTransitioning.value = false;
    }, 50); // Small delay to ensure new image is loaded
  }, imageTransitionDuration / 2);

  setTimeout(() => {
    isTransitioning.value = false;
  }, transitionDuration / 2);
}

function prevProject() {
  if (isTransitioning.value) return;
  console.log("clicked");
  isTransitioning.value = true;
  isImageTransitioning.value = true;

  // Start image fade out
  setTimeout(() => {
    // Change project (this triggers image change)
    currentIndex.value = (currentIndex.value - 1) % projects.length;
    // Start image fade in
    setTimeout(() => {
      isImageTransitioning.value = false;
    }, 65); // Small delay to ensure new image is loaded
  }, imageTransitionDuration / 2);

  setTimeout(() => {
    isTransitioning.value = false;
  }, transitionDuration / 2);
}
</script>

<template>
  <div
    class="project-card"
    :class="{ transitioning: isTransitioning }"
    :style="{ backgroundColor: project.bgColor, color: project.color }"
  >
    <section class="background-circles">
      <ul>
        <li class="circle circle-2"></li>
        <li class="circle circle-3"></li>
      </ul>
    </section>
    <header>
      <h1>A tour of my work</h1>
    </header>

    <!-- Mobile/Tablet Layout (< 900px) -->
    <main v-if="!isDesktop">
      <h2
        class="color-transition"
        :style="{ textDecorationColor: project.uColor }"
      >
        {{ project.title }}
      </h2>
      <img
        :src="project.image"
        :alt="project.title"
        :class="{ 'image-fade-out': isImageTransitioning }"
        class="project-image"
      />

      <section class="content">
        <p>{{ project.desc }}</p>
        <video
          :style="{ border: `1px solid ${project.iColor}` }"
          :key="currentIndex"
          :src="project.video"
          controls
          preload="metadata"
        ></video>
      </section>

      <section class="stack">
        <h2
          class="color-transition"
          :style="{ textDecorationColor: project.uColor }"
        >
          Tech stack
        </h2>
        <ul>
          <li v-for="(tech, i) in project.stack" :key="i">
            <i
              :class="[tech.icon[0], `fa-${tech.icon[1]}`, 'color-transition']"
              :style="{ color: project.iColor, fontSize: '2rem' }"
            ></i>
          </li>
        </ul>
      </section>

      <section class="repo">
        <h3>Check out the repo</h3>
        <a :href="project.gitHub" target="_blank" rel="noopener noreferrer">
          <i
            class="fa-brands fa-github color-transition"
            :style="{ color: project.iColor, fontSize: '2.5rem' }"
          ></i>
        </a>
      </section>

      <section class="btn">
        <svg
          xmlns="http://www.w3.org/2000/svg"
          width="36"
          height="36"
          viewBox="0 0 24 24"
          fill="none"
          stroke="currentColor"
          stroke-width="2"
          stroke-linecap="round"
          stroke-linejoin="round"
          class="lucide lucide-triangle-icon lucide-triangle"
          style="transform: rotate(-90deg)"
          @click="prevProject"
        >
          <path
            d="M13.73 4a2 2 0 0 0-3.46 0l-8 14A2 2 0 0 0 4 21h16a2 2 0 0 0 1.73-3Z"
          />
        </svg>
        <svg
          xmlns="http://www.w3.org/2000/svg"
          width="36"
          height="36"
          viewBox="0 0 24 24"
          fill="none"
          stroke="currentColor"
          stroke-width="2"
          stroke-linecap="round"
          stroke-linejoin="round"
          class="lucide lucide-triangle-icon lucide-triangle"
          style="transform: rotate(90deg)"
          @click="nextProject"
        >
          <path
            d="M13.73 4a2 2 0 0 0-3.46 0l-8 14A2 2 0 0 0 4 21h16a2 2 0 0 0 1.73-3Z"
          />
        </svg>
      </section>

      <footer>
        {{ project.title }} — {{ currentIndex + 1 }} of {{ projects.length }}
      </footer>
    </main>

    <!-- Desktop Layout (>= 900px) -->
    <main v-else class="desktop-layout">
      <div class="desktop-grid">
        <!-- Left Side -->
        <div class="desktop-left">
          <main class="desktop-left-main">
            <h1
              class="color-transition"
              :style="{ textDecorationColor: project.uColor }"
            >
              {{ project.title }}
            </h1>
            <img
              :src="project.image"
              :alt="project.title"
              :class="{ 'image-fade-out': isImageTransitioning }"
              class="project-image"
            />
            <section class="desktop-flex-section">
              <div class="pSize">
                <p>{{ project.desc }}</p>
              </div>
              <!-- Placeholder for second image - you can add another image here if needed -->
              <div class="desktop-second-image" v-if="project.secImage">
                <img
                  :src="project.secImage"
                  :alt="project.title + ' second image'"
                />
              </div>
            </section>
          </main>
        </div>

        <!-- Right Side -->
        <div class="desktop-right">
          <section class="desktop-right-section">
            <video
              :style="{
                border:
                  windowWidth > 900
                    ? `3px solid ${project.iColor}`
                    : `2px solid ${project.iColor}`,
              }"
              :key="currentIndex"
              :src="project.video"
              controls
              preload="metadata"
              class="desktop-video"
            ></video>

            <h2
              class="color-transition"
              :style="{ textDecorationColor: project.uColor }"
            >
              Tech stack
            </h2>
            <ul class="desktop-tech-stack">
              <li v-for="(tech, i) in project.stack" :key="i">
                <i
                  :class="[
                    tech.icon[0],
                    `fa-${tech.icon[1]}`,
                    'color-transition',
                  ]"
                  :style="{ color: project.iColor, fontSize: '3rem' }"
                ></i>
              </li>
            </ul>

            <h2
              class="color-transition"
              :style="{ textDecorationColor: project.uColor }"
            >
              Check out the repo
            </h2>
            <a
              :href="project.gitHub"
              target="_blank"
              rel="noopener noreferrer"
              class="desktop-github"
            >
              <i
                class="fa-brands fa-github color-transition"
                :style="{ color: project.iColor, fontSize: '3rem' }"
              ></i>
            </a>
          </section>
        </div>
      </div>

      <!-- Navigation buttons -->
      <section class="desktop-navigation">
        <svg
          xmlns="http://www.w3.org/2000/svg"
          width="36"
          height="36"
          viewBox="0 0 24 24"
          fill="none"
          stroke="currentColor"
          stroke-width="2"
          stroke-linecap="round"
          stroke-linejoin="round"
          class="lucide lucide-triangle-icon lucide-triangle"
          style="transform: rotate(-90deg)"
          @click="prevProject"
        >
          <path
            d="M13.73 4a2 2 0 0 0-3.46 0l-8 14A2 2 0 0 0 4 21h16a2 2 0 0 0 1.73-3Z"
          />
        </svg>
        <svg
          xmlns="http://www.w3.org/2000/svg"
          width="36"
          height="36"
          viewBox="0 0 24 24"
          fill="none"
          stroke="currentColor"
          stroke-width="2"
          stroke-linecap="round"
          stroke-linejoin="round"
          class="lucide lucide-triangle-icon lucide-triangle"
          style="transform: rotate(90deg)"
          @click="nextProject"
        >
          <path
            d="M13.73 4a2 2 0 0 0-3.46 0l-8 14A2 2 0 0 0 4 21h16a2 2 0 0 0 1.73-3Z"
          />
        </svg>
      </section>

      <footer>
        {{ project.title }} — {{ currentIndex + 1 }} of {{ projects.length }}
      </footer>
    </main>
  </div>
</template>

<style scoped>
@font-face {
  font-family: "RalewayBold";
  src: url("/fonts/Raleway-Bold.ttf") format("truetype");
  font-weight: bold;
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

/* wrapper uses inline background & color from project data */
.project-card {
  width: 100%;
  padding: 1rem;
  min-height: 100vh;
  transition: background-color 0.8s ease-in-out, color 0.8s ease-in-out;
}

.project-card.transitioning {
  transition: background-color 0.4s ease-in-out, color 0.4s ease-in-out;
}

/* Color transition class for elements that change color */
.color-transition {
  transition: color 0.8s ease-in-out, text-decoration-color 0.8s ease-in-out;
}

/* ensure elements use inherited color */
header {
  font-family: "RalewaySemiBold", sans-serif;
  display: flex;
  align-items: center;
  gap: 1rem;
  color: inherit;
  margin-top: 2rem;
  position: relative;
  z-index: 1;
}
header button {
  background: none;
  border: none;
  color: inherit;
  cursor: pointer;
}

header svg {
  cursor: pointer;
  transition: transform 0.2s ease;
}

header svg:hover {
  transform: rotate(90deg) scale(1.1);
}
main {
  margin-top: 2rem;
  position: relative;
  z-index: 1;
}
main h2 {
  font-family: "RalewayBold", sans-serif;
  margin: 1rem 0;
  color: inherit;
  text-decoration: underline;
  transition: color 0.8s ease-in-out, text-decoration-color 0.8s ease-in-out;
}
main img {
  width: 100%;
  height: 150px;
  object-fit: cover;
}

.project-image {
  transition: opacity 0.6s ease-in-out;
  opacity: 1;
}

.project-image.image-fade-out {
  opacity: 0;
}
section {
  display: flex;
}

.content {
  justify-content: space-between;
  margin: 2rem 0;
}

.stack {
  flex-direction: column;
}

.repo {
  flex-direction: column;
  text-align: center;
  align-items: center;
  margin-top: 3rem;
  font-family: "RalewaySemiBold", sans-serif;
  font-size: 1.5rem;
  gap: 1.5rem;
}

.btn {
  display: flex;
  margin-top: 2rem;
  justify-content: center;
  gap: 1rem;
}
section p {
  font-family: "RalewayMedium", sans-serif;
  margin: auto;
  font-size: 1rem;
  color: inherit;
  transition: color 0.8s ease-in-out;
}
section i:hover {
  size: 1.5;
}
img {
  max-width: 100%;
  height: auto;
  display: block;
}
video {
  width: 60%;
  max-width: 300px;
  display: block;
  margin-top: 0.75rem;
  transition: border-color 0.8s ease-in-out;
}
ul {
  list-style: none;
  padding: 0;
  display: flex;
  flex-wrap: wrap;
  margin: 0.5rem 0;
}
li {
  padding: 0.25rem 0.5rem;
  font-size: 0.9rem;
  color: inherit;
  transition: color 0.8s ease-in-out;
}
footer {
  color: inherit;
  font-family: "RalewaySemiBold", sans-serif;
  transition: color 0.8s ease-in-out;
}

/* Background circles animation */
.background-circles {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  overflow: hidden;
  z-index: 0;
  pointer-events: none;
}

.background-circles ul {
  position: relative;
  width: 100%;
  height: 100%;
  margin: 0;
  padding: 0;
}

.circle {
  position: absolute;
  border-radius: 50%;
  opacity: 0.1;
  animation: float 6s ease-in-out infinite;
  filter: blur(6px);
}

.circle-2 {
  width: 120px;
  height: 120px;
  top: 60%;
  right: 15%;
  background: currentColor;
  animation-delay: 2s;
  animation-duration: 10s;
}

.circle-3 {
  width: 60px;
  height: 60px;
  bottom: 20%;
  left: 20%;
  background: currentColor;
  animation-delay: 4s;
  animation-duration: 7s;
}

@keyframes float {
  0%,
  100% {
    transform: translateY(0px) translateX(0px) scale(1);
  }
  25% {
    transform: translateY(-20px) translateX(10px) scale(1.1);
  }
  50% {
    transform: translateY(-10px) translateX(-15px) scale(0.9);
  }
  75% {
    transform: translateY(-30px) translateX(5px) scale(1.05);
  }
}

/* Responsive: mobile-first up to 1024px */
/* Small phones ~360px */
@media (min-width: 360px) {
  .project-card {
    padding: 0.75rem;
  }
  header h1 {
    font-size: 1.25rem;
  }
  main h2 {
    font-size: 1.25rem;
  }
  main img {
    height: 160px;
  }
  footer {
    margin-top: 0.3rem;
  }
}

/* 480px */
@media (min-width: 480px) {
  .project-card {
    padding: 1rem;
  }
  header h1 {
    font-size: 1.4rem;
  }
  main h2 {
    font-size: 1.35rem;
  }
  .content {
    margin: 1.5rem 0;
  }
  .stack {
    margin-bottom: -1rem;
  }
  section p {
    font-size: 2rem;
  }
  main img {
    height: 180px;
  }
  footer {
    margin-top: 0.3rem;
  }
}

/* 600px */
@media (min-width: 600px) {
  header h1 {
    font-size: 1.6rem;
  }
  main h2 {
    font-size: 1.45rem;
  }
  main img {
    height: 200px;
  }
  section p {
    font-size: 2rem;
  }
  .stack {
    margin-bottom: -1rem;
  }
  .btn {
    margin-top: 1rem;
  }
}

/* Tablets 768px */
@media (min-width: 768px) {
  .project-card {
    padding: 1.25rem;
  }
  main {
    margin-top: 2.5rem;
  }
  header h1 {
    font-size: 1.8rem;
  }
  main h2 {
    font-size: 1.6rem;
  }
  section p {
    font-size: 2rem;
  }
  main img {
    height: 240px;
  }
  video {
    width: 60%;
    max-width: 300px;
    margin-left: auto;
    margin-right: auto;
  }
  .repo {
    margin-top: 1rem;
  }
  .stack {
    margin-bottom: -2rem;
  }
  footer {
    font-size: 1rem;
    margin-top: -1.5rem;
  }
}

/* Large tablets up to 1024px */
@media (min-width: 900px) {
  header h1 {
    font-size: 2rem;
  }
  main h2 {
    font-size: 1.8rem;
  }
  main img {
    width: 100%;
    height: 100%;
  }
  video {
    width: 60%;
    max-width: 400px;
    margin-left: auto;
    margin-right: auto;
  }
  .stack {
    margin-bottom: -2rem;
  }
  .pSize {
    width: 30%;
  }

  .desktop-navigation svg {
    font-size: 3rem;
  }
}

@media (min-width: 1024px) {
  .project-card {
    padding: 1.5rem;
  }
  .desktop-flex-section{
    display: flex;
    justify-content: space-between;
  }
  header h1 {
    font-size: 2.2rem;
  }
  main h2 {
    font-size: 2rem;
  }
  section p {
    font-size: 3rem;
    word-break: break-word;
    white-space: normal;
  }
  video {
    width: 65%;
    max-width: 600px;
  }
  .repo {
    margin-top: 3rem;
  }
}

/* Desktop Layout Styles (>= 900px) */
.desktop-layout {
  display: flex;
  flex-direction: column;
  height: 100%;
}

.desktop-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 7rem;
  align-items: start;
}

/* Left Side */
.desktop-left-main {
  display: flex;
  flex-direction: column;
  gap: 2rem;
}

.desktop-left-main h1 {
  font-family: "RalewayBold", sans-serif;
  font-size: 2.5rem;
  margin: 0;
  color: inherit;
  text-decoration: underline;
  transition: color 0.8s ease-in-out, text-decoration-color 0.8s ease-in-out;
}

.desktop-left-main .project-image {
  width: 100%;
  height: 150px;
  object-fit: cover;
  border-radius: 8px;
}

.desktop-flex-section {
  display: flex;
  gap: 2rem;
  align-items: flex-start;
}

.desktop-flex-section p {
  font-family: "RalewayMedium", sans-serif;
  font-size: 2rem;
  margin: 0;
  color: inherit;
  transition: color 0.8s ease-in-out;
  flex: 1;
}

.desktop-second-image {
  width: auto;
  height: 150px;
  background: rgba(255, 255, 255, 0.1);
  border-radius: 8px;
  display: flex;
  align-items: center;
  justify-content: center;
  color: rgba(255, 255, 255, 0.5);
  font-size: 0.9rem;
}

/* Right Side */
.desktop-right {
  margin-top: 6rem;
}
.desktop-right-section {
  width: 70%;
  display: flex;
  flex-direction: column;
  gap: 2rem;
}

.desktop-video {
  width: 100%;
  max-width: 100%;
  transition: border-color 0.8s ease-in-out;
}

.desktop-right-section h2 {
  font-family: "RalewayBold", sans-serif;
  font-size: 1.8rem;
  margin: 0;
  color: inherit;
  text-decoration: underline;
  transition: color 0.8s ease-in-out, text-decoration-color 0.8s ease-in-out;
}

.desktop-tech-stack {
  list-style: none;
  padding: 0;
  display: flex;
  flex-wrap: nowrap;
  gap: 1rem;
  margin: 0;
}

.desktop-tech-stack li {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 60px;
  height: 60px;
  border-radius: 8px;
  transition: color 0.8s ease-in-out;
}

.desktop-github {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  width: 60px;
  height: 60px;
  border-radius: 8px;
  text-decoration: none;
  transition: background-color 0.3s ease;
}

.desktop-github:hover {
  background: rgba(255, 255, 255, 0.2);
}

/* Navigation */
.desktop-navigation {
  display: flex;
  justify-content: center;
  gap: 2rem;
  margin-top: 3rem;
  padding: 2rem 0;
}

.desktop-navigation svg {
  cursor: pointer;
  transition: transform 0.2s ease;
}

.desktop-navigation svg:hover {
  transform: scale(1.1);
}
</style>
