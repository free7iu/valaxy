<script lang="ts" setup>
import { useFrontmatter } from 'valaxy'
import PressButton from './PressButton.vue'

const fm = useFrontmatter()
</script>

<template>
  <div text="center" m="md:t-24 t-10 md:t-20" flex="~ col" justify="center" items="center">
    <ValaxyLogo mb="2" />
    <h1 my="10" text="4xl md:8xl" font="black" class="gradient-text from-purple-800 to-blue-500" bg="gradient-to-r">
      {{ fm.hero.name }}
    </h1>
  </div>

  <h2 m="b-10" text="center 6xl" font="black" leading="tight">
    Next Generation
    <br>
    Static <span class="gradient-text from-blue-500 to-purple-700" bg="gradient-to-r">Blog</span> Framework
  </h2>

  <div p="2" text="center">
    <PressButton
      v-for="action in fm.hero.actions"
      :key="action.link"
      :theme="action.theme"
      :link="action.link"
      :text="action.text"
    />
  </div>

  <br>
</template>
