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
      <img src="@/assets/keygen.png" alt="Imagen adaptada" />

      <!-- Inputs encima de la imagen -->
      <label for="serial" class="input-label serial-label">Hardware Code:</label>
      <input id="serial" v-model="serial" type="text" class="input-field serial" />
      
      <label for="response" class="input-label response-label">Log:</label>
      <input id="response" v-model="response" type="text" class="input-field response" readonly />

      <button @click="generateKey" class="generate-btn">Generate</button>
      <button @click="aboutAction" class="about-btn">About</button>

      <button @click="copyResponse" class="copy-btn">Copy</button>
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
  width: 580px;
  height: 500px;
  max-width: 95vw;
  max-height: 90vh;
  background: #f5f5f7;
  border-radius: 10px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.2);
  display: flex;
  flex-direction: column;
  overflow: hidden;
}

/* Mobile styles */
@media (max-width: 768px) {
  .window {
    width: 90vw;
    height: auto;
    max-width: 90vw;
    aspect-ratio: 580 / 500;
  }
}

@media (max-width: 480px) {
  .window {
    width: 95vw;
    max-width: 95vw;
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

.input-field {
  position: absolute;
  background: rgba(255, 255, 255, 0.9);
  color: black;
  border: 1px solid #ccc;
  padding: 5px;
  font-size: 14px;
  width: 70%; /* Aumentamos el ancho */
  text-align: left;
  transform: translateX(-50%);
  left: 49.5%; /* Centramos completamente */
}

@media (max-width: 768px) {
  .input-field {
    font-size: 16px; /* Prevents zoom on iOS */
    padding: 8px;
    width: 75%;
  }
}





/* Posicionamiento específico de cada input */
.serial {
  top: 65%;
}
.response {
  top: 75%;
}

/* Labels */
.input-label {
  position: absolute;
  color: rgb(255, 255, 255);
  font-size: 14px;
  width: auto; /* Solo ocupa el espacio necesario */
  text-align: left; /* Alinea el texto a la izquierda */
  padding: 2px 5px;
  border-radius: 3px;
  left: 13%; /* Ajusta el margen izquierdo */
  background: rgba(0, 0, 0, 0.6);
  text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.8);
}



.serial-label {
  color: rgb(255, 255, 255);
  
  top: 56%; /* Ajustado para mantenerse dentro del recuadro */
}

.response-label {
  top: 70%; /* Ajustado para que coincida con el input */
}

@media (max-width: 768px) {
  .serial-label {
    top: 54%;
  }
  .response-label {
    top: 68%;
  }
  .serial {
    top: 60%;
  }
  .response {
    top: 74%;
  }
}


button {
  position: absolute;
  background: #e0e0e0;
  color: black;
  border: 2px solid #b0b0b0;
  padding: 5px 10px;
  font-size: 14px;
  cursor: pointer;
  box-shadow: 1px 1px 0px #fff inset, -1px -1px 0px #fff inset, 1px 1px 0px #808080, -1px -1px 0px #808080;
  border-radius: 0; /* Hace que los bordes sean cuadrados */
}


button:active {
  box-shadow: inset 1px 1px 0px #808080, inset -1px -1px 0px #808080, inset 1px 1px 0px #fff, inset -1px -1px 0px #fff;
}

.generate-btn {
  bottom: 2%;
  left: 20%;
}

.about-btn {
  bottom: 2%;
  left: 51%;
  transform: translateX(-50%);
}

.copy-btn {
  bottom: 2%;
  right: 21%;
}

@media (max-width: 768px) {
  button {
    padding: 8px 12px;
    font-size: 12px;
    min-height: 36px;
  }
  
  .generate-btn {
    left: 5%;
  }
  
  .copy-btn {
    right: 5%;
  }
  
  .input-label {
    font-size: 12px;
    left: 10%;
  }
}

@media (max-width: 400px) {
  button {
    padding: 6px 8px;
    font-size: 11px;
  }
  
  .input-field {
    width: 80%;
  }
  
  .input-label {
    left: 8%;
    font-size: 11px;
  }
}

</style>