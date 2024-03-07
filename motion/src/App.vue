<script setup lang="ts">
import { RouterLink, RouterView } from 'vue-router'
import HelloWorld from './components/HelloWorld.vue'
import { MotionDirective as motion, type MotionVariants } from '@vueuse/motion'
import { useMotions } from '@vueuse/motion'
import { onMounted, ref } from 'vue'
import { useDrag, type EventTypes, type State } from '@vueuse/gesture'

const vMotion = motion()

const motions = useMotions()

// Dummy custom event function
const yourCustomEvent = () => {
  // Access the motion instance using useMotions.
  const { customElement } = motions

  console.log('Custom event triggered', customElement)
  // Edit the variant using the motion instance.
  customElement.variant.value = 'custom'
}

const dragHandler = ({ offset: [x, y], movement, dragging }: State['drag']) => {
  const { dragDom } = motions

  if (!dragDom) {
    return
  }
  console.log('dragHandler', x, y, movement)

  // if (!dragging) {
  //   dragDom.apply({
  //     x: 0,
  //     y: 0,
  //     cursor: 'grab'
  //   })

  //   return
  // }

  dragDom.apply({
    x,
    y,
    cursor: 'grabbing'
  })
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
      v-drag="dragHandler"
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

  <div
    class="box"
    v-motion="'dragDom'"
    v-drag="dragHandler"
    :drag-options="{
      filterTaps: true,
      preventWindowScrollY: false
    }"
  >
    drag item
  </div>

  <RouterView />
</template>

<style scoped>
.box {
  position: absolute;
  width: 100px;
  height: 100px;
  background-color: red;
  color: #fff;
  z-index: 10;
}

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
