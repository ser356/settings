<!-- filepath: /Users/ser356/Code/ajust.es/src/components/KeygenComponent.vue -->
<template>
  <div class="image-container">
    <img src="@/assets/keygen.png" alt="Imagen centrada" />
  </div>
</template>

<style scoped>
.image-container {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 90dvh; /* Usa dynamic viewport height para iOS */
  overflow: hidden; /* Evita que el contenido desborde */
}

.image-container img {
  width: auto;
  max-width: 90%;
  max-height: 80dvh; /* Asegura que no desborde la pantalla */
  object-fit: contain;
  padding: 10px;
}

/* Ajustes específicos para móviles */
@media (max-width: 768px) {
  .image-container {
    height: 100vh; /* Asegura que el contenedor ocupe toda la pantalla */
    padding: 0; /* Evita espacios extra */
  }

  .image-container img {
    max-width: 100%; /* Evita que la imagen se haga demasiado grande */
    max-height: 70vh; /* Controla la altura para que no se vea tan pequeña */
  }
}

</style>