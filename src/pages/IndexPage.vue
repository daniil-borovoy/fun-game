<template>
  <q-page class="no-padding no-margin" :style-fn="styleFn">
    <ExampleComponent :height="gameHeight"/>
  </q-page>
</template>

<script setup lang="ts">
import ExampleComponent from 'components/ExampleComponent.vue';
import {ref} from 'vue';

const gameHeight = ref();

const styleFn = (offset: number, height: number) => {
  gameHeight.value = height - (offset);
  console.log(height - (offset))
};
</script>
