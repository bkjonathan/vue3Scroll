<template>
  <div class="loader" :class="{show}">
    <div class="circle"></div>
    <div class="circle"></div>
    <div class="circle"></div>
  </div>
</template>

<script>
export default {
  name: "Loader",
    props:['show']
};
</script>

<style scoped></style>
