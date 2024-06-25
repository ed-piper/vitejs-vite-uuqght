<script setup>
import { ref, watch } from 'vue';
import HelloWorld from './components/HelloWorld.vue';
import { rgb } from 'culori';
import { differenceCiede2000, converter } from 'culori';

let pureColor = ref('');
let gradientColor = ref('');
let pureColor2 = ref('');
let gradientColor2 = ref('');
let difference = ref(0);

function rgbToObject(rgbString) {
  if (typeof rgbString !== 'string' || rgbString.length === 0) {
    return null;
  }
  console.log(rgbString);
  let rgbValues = rgbString.match(/\d+/g);
  console.log(rgbValues);
  return {
    mode: 'rgb',
    r: parseInt(rgbValues[0]),
    g: parseInt(rgbValues[1]),
    b: parseInt(rgbValues[2]),
    alpha: 1,
  };
}

watch([pureColor, pureColor2], () => {
  let lab = converter('lab');
  let color1 = lab('#f0f0f0');
  let color2 = lab('#ff0000');
  const newDiff = differenceCiede2000(color1, color2);
  difference.value = newDiff;
});
</script>

<template>
  <div class="container">
    <div>
      <p>Picker 1</p>
      <color-picker
        v-model:pureColor="pureColor"
        v-model:gradientColor="gradientColor"
      />
      <p>Color: {{ pureColor }}</p>
    </div>
    <div>
      <p>Picker 2</p>
      <color-picker
        v-model:pureColor="pureColor2"
        v-model:gradientColor="gradientColor2"
      />
      <p>Color: {{ pureColor2 }}</p>
    </div>
    <div>
      <p>Difference: {{ difference }}</p>
    </div>
  </div>
</template>

<style scoped>
p {
  color: black;
}
.container {
  background-color: white;
}
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
