<template>
  <div class="container">
    <img src="https://source.unsplash.com/storm-trooper-head-decor-xv56z71BEcI" class="hero-image"/>
    <h1>Never <span class="hero-title-linear">tell me the odds</span></h1>
    <a href="#table" class="hero-button"><ArrowIcon class="hero-icon"/></a>
  </div>
</template>
<script setup>
import ArrowIcon from './icons/IconArrow.vue'
defineProps({
  msg: {
    type: String,
    required: true
  }
})
</script>

<style scoped>
.container {
  position: relative;

}
.container::before {
  background-color: var(--color-dark-gray);
  content: '';
  width: 100%;
  height: 100%;
  opacity: 0.05;
  position: absolute;
}

h1 {
  color: var(--color-lynch);
  font-weight: 900;
  font-size: 2rem;
  top: 3rem;
  padding: 1rem;
  position: absolute;
  text-transform: uppercase;
  text-align: center;
  width: 100%;
}
.hero-title-linear {
  font-weight: 900;
}
.hero-image {
  height: 100vh;
  width: 100%;
  object-fit: cover
}

.hero-button {
  align-items: center;
  background-color: var(--color-black);
  border: none;
  border-radius: 50%;
  bottom: 2rem;
  color: var(--color-white-soft);
  display: flex;
  height: 4rem;
  justify-content: center;
  padding: 1.5rem;
  position: absolute;
  right: 2rem;
  width: 4rem;
}

.hero-icon {
  display: flex;
  justify-content: center;
  text-align:center;
  vertical-align: middle;
}

@media (min-width: 767px) {
h1 {
  font-size: 6rem;
  left: 50%;
  text-align: right;
  width: fit-content;
}

.hero-title-linear {
  -webkit-text-fill-color: transparent;
  -webkit-text-stroke: 3px;
}

}

@media (min-width: 1024px) {
  h1 {
    color: var(--color-white);
    font-size: 8rem;
    padding: 2rem;
    top: 3rem;
  }
  .hero-button {
    height: 5rem;
    padding: 1.75rem;
    width: 5rem;
  }
}
</style>