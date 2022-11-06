<template>
  <div v-if="posts.length > 0">
    <h3>Список постів</h3>
    <transition-group name="posts-list">
      <post-item
          v-for="post in posts"
          :post="post"
          :key="post.id"
          @remove="$emit('remove', post)"
      />
    </transition-group>
  </div>
  <h2 v-else style="color: red">Список постів пустий</h2>
</template>

<script>
import PostItem from "@/components/PostItem";

export default {
  components: {PostItem},
  props: {
    posts: {
      type: Array,
      required: true
    }
  }
}
</script>

<style scoped>
.posts-list-move,
.posts-list-enter-active,
.posts-list-leave-active {
  transition: all 0.4s ease;
}
.posts-list-enter-from,
.posts-list-leave-to {
  opacity: 0;
  transform: translateX(30px);
}
</style>