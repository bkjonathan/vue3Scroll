<template>
  <div class="post">
    <div class="post-number">{{ post.id }}</div>
    <div class="post-info">
      <div class="post-title">{{ post.title }}</div>
      <div class="post-body">
        {{ post.body }}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Posts",
  props: ["post"]
};
</script>

<style scoped></style>
