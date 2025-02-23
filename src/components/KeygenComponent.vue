<template>
  <div class="window" ref="window">
    <div class="title-bar" @mousedown="startDrag">
      <div class="buttons">
        <span class="close"></span>
        <span class="minimize"></span>
        <span class="maximize"></span>
      </div>
      <span class="title">ajustes.app</span>
    </div>

    <div class="content">
      <img src="@/assets/keygen.png" alt="Imagen adaptada" />

      <!-- Inputs encima de la imagen -->
      <input v-model="serial" type="text" class="input-field serial" placeholder="Hardware Code" />
      <input v-model="response" type="text" class="input-field response" readonly />

      <button @click="generateKey" class="generate-btn">Generate</button>
      <button @click="copyResponse" class="copy-btn">Copy</button>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";

const window = ref(null);
const serial = ref("");
const challenge = ref("");
const response = ref("");

const offsetX = ref(0);
const offsetY = ref(0);

const generateKey = () => {
  response.value = "ABCD-1234-EFGH-5678"; // Simulación de respuesta
};

const copyResponse = () => {
  navigator.clipboard.writeText(response.value);
};

// --- DRAG ---
const startDrag = (event) => {
  event.preventDefault();
  offsetX.value = event.clientX - window.value.offsetLeft;
  offsetY.value = event.clientY - window.value.offsetTop;

  document.addEventListener("mousemove", drag);
  document.addEventListener("mouseup", stopDrag);
};

const drag = (event) => {
  if (window.value) {
    window.value.style.left = `${event.clientX - offsetX.value}px`;
    window.value.style.top = `${event.clientY - offsetY.value}px`;
  }
};

const stopDrag = () => {
  document.removeEventListener("mousemove", drag);
  document.removeEventListener("mouseup", stopDrag);
};
</script>

<style scoped>
.window {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 580px;
  height: 500px;
  background: #f5f5f7;
  border-radius: 10px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.2);
  display: flex;
  flex-direction: column;
  overflow: hidden;
}

.title-bar {
  display: flex;
  align-items: center;
  justify-content: space-between;
  background: #ddd;
  padding: 8px;
  font-weight: bold;
  font-size: 14px;
  cursor: grab;
}

.title-bar .buttons {
  display: flex;
  gap: 6px;
}

.title-bar .buttons span {
  width: 12px;
  height: 12px;
  border-radius: 50%;
  display: inline-block;
}

.close {
  background: #ff5f56;
}
.minimize {
  background: #ffbd2e;
}
.maximize {
  background: #28c940;
}

.content {
  flex-grow: 1;
  display: flex;
  justify-content: center;
  align-items: center;
  position: relative;
}

.content img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

/* --- INPUTS ESTILIZADOS --- */
.input-field {
  position: absolute;
  background: rgba(0, 0, 0, 0.7);
  color: white;
  border: 1px solid white;
  padding: 5px;
  font-size: 14px;
  width: 70%;
  text-align: center;
  left: 50%;
  transform: translateX(-50%);
}

/* Posicionamiento específico de cada input */
.serial {
  top: 65%;
}
.response {
  top: 75%;
}

/* Botones */
button {
  position: absolute;
  background: #555;
  color: white;
  border: none;
  padding: 5px 10px;
  font-size: 14px;
  cursor: pointer;
}

button:hover {
  background: #777;
}

.generate-btn {
  bottom: 2%;
  left: 30%;
}

.copy-btn {
  bottom: 2%;
  left: 60%;
}
</style>
