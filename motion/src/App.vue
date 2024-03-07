<script setup lang="ts">
import { RouterLink, RouterView } from 'vue-router'
import HelloWorld from './components/HelloWorld.vue'
import { MotionDirective as motion, type MotionVariants } from '@vueuse/motion'
import { useMotions } from '@vueuse/motion'
import { onMounted } from 'vue'

const vMotion = motion()

// Dummy custom event function
const yourCustomEvent = () => {
  // Access the motion instance using useMotions.
  const { customElement } = useMotions()

  console.log('Custom event triggered', customElement)
  // Edit the variant using the motion instance.
  customElement.variant.value = 'custom'
}
</script>

<template>
  <header>
    <img
      alt="Vue logo"
      class="logo"
      src="@/assets/logo.svg"
      width="125"
      height="125"
      @click="yourCustomEvent"
    />

    <div
      class="wrapper"
      v-motion="'customElement'"
      :initial="{ opacity: 0 }"
      :enter="{
        opacity: 1,
        transition: { repeat: Infinity, repeatType: 'mirror' }
      }"
      :variants="{ custom: { scale: 2 } }"
      :delay="1000"
    >
      <HelloWorld msg="You did it!" />

      <nav>
        <RouterLink to="/">Home</RouterLink>
        <RouterLink to="/about">About</RouterLink>
      </nav>
    </div>
  </header>

  <RouterView />
</template>

<style scoped>
header {
  line-height: 1.5;
  max-height: 100vh;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

nav {
  width: 100%;
  font-size: 12px;
  text-align: center;
  margin-top: 2rem;
}

nav a.router-link-exact-active {
  color: var(--color-text);
}

nav a.router-link-exact-active:hover {
  background-color: transparent;
}

nav a {
  display: inline-block;
  padding: 0 1rem;
  border-left: 1px solid var(--color-border);
}

nav a:first-of-type {
  border: 0;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }

  nav {
    text-align: left;
    margin-left: -1rem;
    font-size: 1rem;

    padding: 1rem 0;
    margin-top: 1rem;
  }
}
</style>
