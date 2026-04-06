<template>
  <div class="window" ref="window">
    <div class="title-bar" @mousedown="startDrag" @touchstart="startDrag">
      <div class="buttons">
        <span class="close"></span>
        <span class="minimize"></span>
        <span class="maximize"></span>
      </div>
    </div>

    <div class="content">
      <div class="image-container">
        <img src="@/assets/keygen.png" alt="Imagen adaptada" />
      </div>
      
      <div class="form-panel">
        <div class="field-group">
          <label class="field-label">Hardware Code :</label>
          <input v-model="serial" type="text" class="input-field" />
        </div>
        
        <div class="field-group">
          <label class="field-label">Log :</label>
          <input v-model="response" type="text" class="input-field" readonly />
        </div>
        
        <div class="button-row">
          <button @click="generateKey">Generate</button>
          <button @click="aboutAction">About</button>
          <button @click="copyResponse">Copy</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
import { onMounted } from "vue";
import music from "@/assets/keygen.mp3";


const playAudio = () => {
  const audio = new Audio(music);
  audio.play().catch(error => console.error("Error playing audio:", error));
};

onMounted(() => {
  document.addEventListener("click", () => {
    playAudio();
  }, { once: true }); // Se ejecuta una sola vez
});

const window = ref(null);
const serial = ref("");
const response = ref("");

const offsetX = ref(0);
const offsetY = ref(0);

const generateKey = () => {
  response.value = "AJUSTES-AJUSTES-AJUSTES"; // Simulación de respuesta
};

const copyResponse = () => {
  navigator.clipboard.writeText(response.value);
};
const aboutAction = () => {
  alert("⚙️ 2025");
};

// --- DRAG (mouse + touch) ---
const getClientPos = (event) => {
  if (event.touches && event.touches.length > 0) {
    return { x: event.touches[0].clientX, y: event.touches[0].clientY };
  }
  return { x: event.clientX, y: event.clientY };
};

const startDrag = (event) => {
  event.preventDefault();
  const pos = getClientPos(event);
  offsetX.value = pos.x - window.value.offsetLeft;
  offsetY.value = pos.y - window.value.offsetTop;

  document.addEventListener("mousemove", drag);
  document.addEventListener("mouseup", stopDrag);
  document.addEventListener("touchmove", drag, { passive: false });
  document.addEventListener("touchend", stopDrag);
};

const drag = (event) => {
  if (window.value) {
    event.preventDefault();
    const pos = getClientPos(event);
    window.value.style.left = `${pos.x - offsetX.value}px`;
    window.value.style.top = `${pos.y - offsetY.value}px`;
  }
};

const stopDrag = () => {
  document.removeEventListener("mousemove", drag);
  document.removeEventListener("mouseup", stopDrag);
  document.removeEventListener("touchmove", drag);
  document.removeEventListener("touchend", stopDrag);
};
</script>

<style scoped>
.window {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 420px;
  background: #f5f5f5;
  border-radius: 14px;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.2);
  display: flex;
  flex-direction: column;
  overflow: hidden;
}

@media (max-width: 768px) {
  .window {
    width: 90vw;
    max-width: 420px;
  }
}

.title-bar {
  display: flex;
  align-items: center;
  background: linear-gradient(to bottom, #e8e8e8, #d0d0d0);
  padding: 12px 14px;
  cursor: grab;
  border-bottom: 1px solid #b0b0b0;
}

.title-bar .buttons {
  display: flex;
  gap: 8px;
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
  display: flex;
  flex-direction: column;
}

.image-container {
  width: 100%;
  background: #000;
}

.image-container img {
  width: 100%;
  height: auto;
  display: block;
}

.form-panel {
  background: #1e1e1e;
  padding: 16px 20px;
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.field-group {
  display: flex;
  flex-direction: column;
  gap: 4px;
}

.field-label {
  color: #ffffff;
  font-size: 13px;
  font-family: -apple-system, BlinkMacSystemFont, 'SF Pro Text', sans-serif;
}

.input-field {
  background: #0a0a0a;
  color: #ffffff;
  border: 1px solid #3a3a3a;
  border-radius: 6px;
  padding: 8px 10px;
  font-size: 14px;
  font-family: -apple-system, BlinkMacSystemFont, 'SF Pro Text', sans-serif;
  width: 100%;
  box-sizing: border-box;
}

.input-field:focus {
  outline: none;
  border-color: #0a84ff;
  box-shadow: 0 0 0 3px rgba(10, 132, 255, 0.3);
}

@media (max-width: 768px) {
  .input-field {
    font-size: 16px;
    padding: 12px;
  }
  
  .field-label {
    font-size: 14px;
  }
  
  .form-panel {
    padding: 20px;
    gap: 14px;
  }
}

.button-row {
  display: flex;
  justify-content: center;
  gap: 12px;
  margin-top: 10px;
}

button {
  background: #3a3a3a;
  color: #ffffff;
  border: none;
  border-radius: 8px;
  padding: 8px 20px;
  font-size: 13px;
  font-family: -apple-system, BlinkMacSystemFont, 'SF Pro Text', sans-serif;
  cursor: pointer;
  min-width: 80px;
  transition: all 0.15s;
}

button:hover {
  background: #4a4a4a;
}

button:active {
  background: #2a2a2a;
}

@media (max-width: 768px) {
  button {
    padding: 12px 16px;
    font-size: 14px;
    min-width: 70px;
    flex: 1;
  }
  
  .button-row {
    gap: 10px;
  }
}
</style>