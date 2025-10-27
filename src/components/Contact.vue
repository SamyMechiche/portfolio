<script setup>
import { ref } from "vue";
import emailjs from "@emailjs/browser";

const props = defineProps({
  open: { type: Boolean, default: false },
});

const emit = defineEmits(["close", "sent"]);

const formRef = ref(null);
const name = ref("");
const email = ref("");
const message = ref("");
const loading = ref(false);
const error = ref("");
const success = ref("");

function resetState() {
  name.value = "";
  email.value = "";
  message.value = "";
  error.value = "";
  success.value = "";
}

async function sendEmail() {
  error.value = "";
  success.value = "";

  if (!name.value || !email.value || !message.value) {
    error.value = "Please fill in all fields.";
    return;
  }

  loading.value = true;
  try {
    const serviceId = import.meta.env.VITE_EMAILJS_SERVICE_ID;
    const templateId = import.meta.env.VITE_EMAILJS_TEMPLATE_ID;
    const publicKey = import.meta.env.VITE_EMAILJS_PUBLIC_KEY;

    if (!serviceId || !templateId || !publicKey) {
      throw new Error("Missing EmailJS environment variables");
    }

    await emailjs.send(
      serviceId,
      templateId,
      {
        name: name.value,
        email: email.value,
        time: new Date().toLocaleString(),
        message: message.value,
      },
      { publicKey }
    );

    success.value = "Message sent! I'll get back to you soon.";
    emit("sent");
    resetState();
  } catch (e) {
    error.value = "Failed to send. Please try again later.";
    // eslint-disable-next-line no-console
    console.error(e);
  } finally {
    loading.value = false;
  }
}

function closeModal() {
  resetState();
  emit("close");
}
</script>

<template>
  <transition name="fade">
    <div v-if="open" class="overlay" @click.self="closeModal">
      <div class="modal">
        <header class="modal-header">
          <h3>Contact me</h3>
          <button class="icon-btn" @click="closeModal" aria-label="Close">
            ×
          </button>
        </header>

        <form ref="formRef" class="form" @submit.prevent="sendEmail">
          <label>
            <span>Name</span>
            <input v-model="name" type="text" placeholder="Your name" />
          </label>
          <label>
            <span>Email</span>
            <input v-model="email" type="email" placeholder="you@email.com" />
          </label>
          <label>
            <span>Message</span>
            <textarea
              v-model="message"
              rows="5"
              placeholder="How can I help?"
            />
          </label>

          <button class="submit" type="submit" :disabled="loading">
            {{ loading ? "Sending..." : "Send" }}
          </button>

          <p v-if="error" class="msg error">{{ error }}</p>
          <p v-if="success" class="msg success">{{ success }}</p>
        </form>
      </div>
    </div>
  </transition>
</template>

<style scoped>
@font-face {
  font-family: "RalewayBold";
  src: url("/fonts/Raleway-Bold.ttf") format("truetype");
  font-weight: bold;
  font-style: normal;
}
@font-face {
  font-family: "RalewayMedium";
  src: url("/fonts/Raleway-Medium.ttf") format("truetype");
  font-weight: 500;
  font-style: normal;
}

header h3 {
  font-family: "RalewayBold", sans-serif;
}

label input {
  font-family: "RalewayMedium", sans-serif;
}
.overlay {
  position: fixed;
  inset: 0;
  background: rgba(0, 0, 0, 0.55);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 50;
}

.modal {
  width: min(560px, 92vw);
  background: #15151e;
  color: #fff;
  border: 1px solid #2a2a3a;
  border-radius: 10px;
  box-shadow: 0 10px 40px rgba(0, 0, 0, 0.35);
}

.modal-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 1rem 1.25rem;
  border-bottom: 1px solid #2a2a3a;
}

.icon-btn {
  background: transparent;
  color: inherit;
  border: none;
  font-size: 1.5rem;
  cursor: pointer;
}

.form {
  display: grid;
  gap: 0.75rem;
  padding: 1rem 1.25rem 1.25rem;
}

label {
  display: grid;
  gap: 0.4rem;
}

label span {
  font-family: "RalewaySemiBold", sans-serif;
  font-size: 0.9rem;
}

input,
textarea {
  font-family: "RalewaySemiBold", sans-serif;
  background: #0f0f18;
  color: #fff;
  border: 1px solid #2a2a3a;
  border-radius: 8px;
  padding: 0.7rem 0.9rem;
  outline: none;
}

input:focus,
textarea:focus {
  border-color: #5aa7ff;
  box-shadow: 0 0 0 3px rgba(90, 167, 255, 0.2);
}

.submit {
  margin-top: 0.5rem;
  background: #5aa7ff;
  color: #0d0d1f;
  border: none;
  border-radius: 8px;
  padding: 0.75rem 1rem;
  font-family: "RalewayBold", sans-serif;
  cursor: pointer;
}

.submit:disabled {
  opacity: 0.7;
  cursor: not-allowed;
}

.msg {
  margin: 0.25rem 0 0;
  font-size: 0.9rem;
}

.msg.error {
  color: #ff6b6b;
}
.msg.success {
  color: #7cd992;
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.15s ease;
}
.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>
