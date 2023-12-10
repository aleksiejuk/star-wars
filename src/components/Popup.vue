<template>
  <div v-if="isVisible" class="overlay"></div>
  <div v-if="isVisible" class="popup-container">
    <slot name="button"></slot>
    <div class="popup-content">
      <h2>{{ content.name }}</h2>
      <p>Diameter: {{ content.diameter }}</p>
      <p>Climate: {{ content.climate }}</p>
      <p>Population: {{ content.population }}</p>
    </div>
  </div>
</template>

<script setup>

const { isVisible, content } = defineProps(['isVisible', 'content']);

</script>

<style scoped>
.overlay {
  background-color: rgba(0, 0, 0, 0.5);
  height: 100%;
  left: 0;
  position: fixed;
  top: 0;
  width: 100%;
  z-index:10;
}

.popup-container {
  align-items: center;
  background-color: var(--color-white);
  border-radius: 50%;
  box-shadow:
    inset 0 0 50px var(--color-white),      
    inset 20px 0 80px var(--color-magenta),   
    inset -20px 0 80px var(--color-cyan), 
    inset 20px 0 300px var(--color-magenta),  
    inset -20px 0 300px var(--color-cyan),
    0 0 50px var(--color-white),           
    -10px 0 80px var(--color-magenta),        
    10px 0 80px var(--color-cyan);         
  display: flex;
  height: 20rem;
  justify-content: center;
  left: 50%;
  padding: 2rem;
  position: fixed;
  text-align: center;
  top: 50%;
  transform: translate(-50%, -50%);
  width: 20rem;
  z-index: 20;
}

.popup-content {
  color: var(--color-lynch);
  text-align: center;
}

@media (min-width: 767px) {
  .popup-container {
    height: 30rem;
    padding: 5rem;
    width: 30rem;
  }
}
</style>

