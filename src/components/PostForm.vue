<template>
  <div>
    <form @submit.prevent>
      <h4>Створення поста</h4>
      <my-input
          type="text"
          v-model="post.title"
          placeholder="Назва"
      />
      <my-input
          type="text"
          v-model="post.body"
          placeholder="Опис"
      />
      <my-button
          class="form-btn"
          @click="createPost"
      >
        Створити
      </my-button>
    </form>
  </div>
</template>

<script>
import MyInput from "@/components/UI/MyInput";
export default {
  components: {MyInput},
  data() {
    return {
      post: {
        title: '',
        body: ''
      }
    }
  },
  methods:{
    createPost() {
      this.post.id = Date.now();
      this.$emit('create', this.post);
      this.post = {
        title: '',
        body: ''
      }
    }
  }
}
</script>

<style scoped>
form {
  display: flex;
  flex-direction: column;
}

.form-btn{
  align-self: flex-end;
  margin-top: 15px;
}
</style>