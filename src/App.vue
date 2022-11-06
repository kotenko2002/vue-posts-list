<template>
  <div class="app">
    <my-input
      v-model="searchValue"
      placeholder="Пошук..."/>
    <h1>Сторінка з постами</h1>
    <div class="app-btn">
      <my-button
          @click="showModal"
      >
        Створити пост
      </my-button>
      <my-select
        v-model="sortValue"
        :options="sortOptions"/>
    </div>
    <my-modal v-model:show="modalVisible">
      <post-form
          @create="createPost"/>
    </my-modal>

    <post-list
        :posts="sortedAndSearchedPosts"
        @remove="removePost"
        v-if="!isPostsLoading"
    />
    <div v-else>Іде завантаження</div>
    <div class="page-wrapper">
      <div
          v-for="pageNumber in totalPages"
          :key="pageNumber"
          class="page"
          :class="{'current-page': pageNumber == page}"
          @click="changePage(pageNumber)">
        {{pageNumber}}
      </div>
    </div>
  </div>
</template>

<script>
  import PostForm from "@/components/PostForm";
  import PostList from "@/components/PostList";
  import axios from 'axios';
  import MyButton from "@/components/UI/MyButton";
  import MyInput from "@/components/UI/MyInput";

  export default {
    components: {
      MyInput,
      MyButton,
      PostList,
      PostForm
    },
    data() {
      return {
        posts: [],
        modalVisible: false,
        isPostsLoading: false,
        page: 1,
        limit : 10,
        totalPages: 0,
        searchValue: '',
        sortValue: '',
        sortOptions: [
          {value: 'title', name: 'По назві'},
          {value: 'body', name: 'По змісту'},
        ]
      }
    },
    methods : {
      createPost(post) {
        this.posts.unshift(post);
        this.modalVisible = false;
      },
      removePost(post) {
        this.posts = this.posts.filter(p => p.id !== post.id)
      },
      showModal() {
        this.modalVisible = true;
      },
      changePage(pageNumber) {
        this.page = pageNumber;
      },
      async fetchPosts() {
        try {
          this.isPostsLoading = true;
          const response = await axios.get('https://jsonplaceholder.typicode.com/posts',{
            params: {
              _page: this.page,
              _limit: this.limit
            }
          });

          this.totalPages = Math.ceil(response.headers['x-total-count'] / this.limit);
          this.posts = response.data;
        } catch (e){
          alert('Сталася помилка')
        } finally {
          this.isPostsLoading = false;
        }
      }
    },
    mounted() {
      this.fetchPosts();
    },
    computed: {
      sortedPosts() {
        return [...this.posts].sort((post1, post2) => post1[this.sortValue]?.localeCompare(post2[this.sortValue]))
      },
      sortedAndSearchedPosts() {
        return this.sortedPosts.filter(post => post.title.toLowerCase().includes(this.searchValue.toLowerCase()))
      }
    },
    watch: {
      page() {
        this.fetchPosts();
      }
    }
  }
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.app {
  padding: 20px;
}

.app-btn {
  margin: 15px 0;
  display: flex;
  justify-content: space-between;
}

.page-wrapper {
  display: flex;
  margin-top: 15px;
}

.page {
  border: 1px solid black;
  padding: 10px;
}

.current-page {
  border: 3px solid teal;
}
</style>
