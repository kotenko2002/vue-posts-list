<template>
  <div class="post">
    <div>
      <div><strong>Назва:</strong> {{post.title}}</div>
      <div><strong>Опис:</strong> {{post.body}}</div>
    </div>
    <div class="post-btn">
      <my-button
        @click="$emit('remove', post)">
          Видалити
      </my-button>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    post:{
      type: Object,
      required: true
    }
  }
}
</script>

<style scoped>
.post {
  padding: 15px;
  margin-top: 15px;
  border: 2px solid teal;
  display: flex;
  align-items: center;
  justify-content: space-between;
}
</style>