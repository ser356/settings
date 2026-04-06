<template>
  <div class="window" ref="window" :class="{ 'is-mobile': isMobile }">
    <div class="title-bar" @mousedown="startDrag" @touchstart="startDrag">
      <div class="buttons">
        <span class="close"></span>
        <span class="minimize"></span>
        <span class="maximize"></span>
      </div>
    </div>

    <div class="content">
      <img src="@/assets/keygen.png" alt="Imagen adaptada" />
    </div>

    <div class="form-area">
      <div class="form-group">
        <label for="serial" class="input-label">Hardware Code:</label>
        <input id="serial" v-model="serial" type="text" class="input-field" />
      </div>
      <div class="form-group">
        <label for="response" class="input-label">Log:</label>
        <input id="response" v-model="response" type="text" class="input-field" readonly />
      </div>
      <div class="button-row">
        <button @click="generateKey" class="generate-btn">Generate</button>
        <button @click="aboutAction" class="about-btn">About</button>
        <button @click="copyResponse" class="copy-btn">Copy</button>
      </div>
    </div>

    <!-- Resize handle for mobile -->
    <div
      v-if="isMobile"
      class="resize-handle"
      @touchstart="startResize"
    ></div>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted, computed } from "vue";
import music from "@/assets/keygen.mp3";

const playAudio = () => {
  const audio = new Audio(music);
  audio.play().catch(error => console.error("Error playing audio:", error));
};

const windowWidth = ref(globalThis.innerWidth);
const isMobile = computed(() => windowWidth.value <= 768);

const onResize = () => {
  windowWidth.value = globalThis.innerWidth;
};

onMounted(() => {
  document.addEventListener("click", () => {
    playAudio();
  }, { once: true });
  globalThis.addEventListener("resize", onResize);
});

onUnmounted(() => {
  globalThis.removeEventListener("resize", onResize);
});

const window = ref(null);
const serial = ref("");
const response = ref("");

const offsetX = ref(0);
const offsetY = ref(0);

const generateKey = () => {
  response.value = "AJUSTES-AJUSTES-AJUSTES";
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

// --- RESIZE (touch for mobile) ---
const resizeStartX = ref(0);
const resizeStartY = ref(0);
const resizeStartW = ref(0);
const resizeStartH = ref(0);

const startResize = (event) => {
  event.preventDefault();
  event.stopPropagation();
  const pos = getClientPos(event);
  resizeStartX.value = pos.x;
  resizeStartY.value = pos.y;
  resizeStartW.value = window.value.offsetWidth;
  resizeStartH.value = window.value.offsetHeight;

  document.addEventListener("touchmove", resizeDrag, { passive: false });
  document.addEventListener("touchend", stopResize);
};

const resizeDrag = (event) => {
  if (window.value) {
    event.preventDefault();
    const pos = getClientPos(event);
    const newW = resizeStartW.value + (pos.x - resizeStartX.value);
    const newH = resizeStartH.value + (pos.y - resizeStartY.value);
    window.value.style.width = `${Math.max(260, newW)}px`;
    window.value.style.height = `${Math.max(220, newH)}px`;
  }
};

const stopResize = () => {
  document.removeEventListener("touchmove", resizeDrag);
  document.removeEventListener("touchend", stopResize);
};
</script>

<style scoped>
.window {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 580px;
  max-width: 95vw;
  max-height: 90vh;
  background: #555;
  border-radius: 10px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.2);
  display: flex;
  flex-direction: column;
  overflow: hidden;
  resize: both;
  min-width: 320px;
  min-height: 280px;
}

@media (max-width: 768px) {
  .window {
    position: fixed;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    width: 92vw;
    max-width: 92vw;
    max-height: 90vh;
    resize: none;
  }
}

@media (max-width: 480px) {
  .window {
    width: 96vw;
    max-width: 96vw;
  }
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
  flex-shrink: 0;
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
  flex-shrink: 1;
  min-height: 0;
  overflow: hidden;
}

.content img {
  width: 100%;
  height: 100%;
  display: block;
  object-fit: cover;
}

.form-area {
  flex-shrink: 0;
  background: #555;
  padding: 12px 16px;
  display: flex;
  flex-direction: column;
  gap: 6px;
}

.form-group {
  display: flex;
  flex-direction: column;
  gap: 2px;
}

.input-label {
  color: rgb(255, 255, 255);
  font-size: 14px;
  text-align: left;
  padding: 0 2px;
}

.input-field {
  background: rgba(255, 255, 255, 0.9);
  color: black;
  border: 1px solid #ccc;
  padding: 6px 8px;
  font-size: 14px;
  width: 100%;
  text-align: left;
  box-sizing: border-box;
}

@media (max-width: 768px) {
  .input-field {
    font-size: 16px;
    padding: 8px 10px;
  }
}

.button-row {
  display: flex;
  justify-content: space-between;
  gap: 8px;
  margin-top: 6px;
}

button {
  background: #e0e0e0;
  color: black;
  border: 2px solid #b0b0b0;
  padding: 6px 16px;
  font-size: 14px;
  cursor: pointer;
  box-shadow: 1px 1px 0px #fff inset, -1px -1px 0px #fff inset, 1px 1px 0px #808080, -1px -1px 0px #808080;
  border-radius: 0;
  flex: 1;
}

button:active {
  box-shadow: inset 1px 1px 0px #808080, inset -1px -1px 0px #808080, inset 1px 1px 0px #fff, inset -1px -1px 0px #fff;
}

@media (max-width: 768px) {
  button {
    padding: 8px 12px;
    font-size: 13px;
    min-height: 36px;
  }

  .input-label {
    font-size: 13px;
  }
}

@media (max-width: 400px) {
  button {
    padding: 6px 8px;
    font-size: 12px;
  }

  .input-field {
    font-size: 14px;
    padding: 6px 8px;
  }

  .input-label {
    font-size: 12px;
  }

  .form-area {
    padding: 10px 12px;
  }
}

/* Resize handle for mobile */
.resize-handle {
  position: absolute;
  bottom: 0;
  right: 0;
  width: 24px;
  height: 24px;
  cursor: nwse-resize;
  background: linear-gradient(
    135deg,
    transparent 40%,
    #888 40%,
    #888 45%,
    transparent 45%,
    transparent 55%,
    #888 55%,
    #888 60%,
    transparent 60%,
    transparent 70%,
    #888 70%,
    #888 75%,
    transparent 75%
  );
  z-index: 10;
  touch-action: none;
}
</style>