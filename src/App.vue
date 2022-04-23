<template>
  <div class="main">
    <div class="main-terminal">
      <button @click="fetchPosts" class="upload-post-btn btn">
        Загрузить посты
      </button>
      <button @click="showCreationWindow" class="add-post-btn btn">
        Добавить пост
      </button>
    </div>
    <post-sorting v-model="selectedSort" :options="sortOption" />
    <post-create-window @click="hideWindow" :show="creationWindow">
      <post-create @add="addPost" />
    </post-create-window>
    <post-list @remove="removePost" @edit="editPost" :posts="sortedPosts" />
    <div class="pages-wrapper">
      <div
        @click="changePage(pageNumber)"
        v-for="pageNumber in totalPages"
        :key="pageNumber"
        :class="{ 'current-page': page === pageNumber }"
        class="page"
      >
        {{ pageNumber }}
      </div>
    </div>
  </div>
</template>

<script>
import PostList from "@/components/PostList";
import PostCreate from "@/components/PostCreate";
import PostCreateWindow from "@/components/PostCreateWindow";
import PostSorting from "@/components/PostSorting";
import axios from "axios";

export default {
  data() {
    return {
      posts: [],
      creationWindow: false,
      selectedSort: "",
      sortOption: [
        { value: "title", name: "По названию" },
        { value: "body", name: "По содержимому" },
      ],
      page: 1,
      limit: 5,
      totalPages: 0,
    };
  },
  components: { PostList, PostCreate, PostCreateWindow, PostSorting },
  methods: {
    addPost(post) {
      this.posts.push(post);
      this.creationWindow = false;
    },
    editPost(post) {
      const isPost = this.posts.find((i) => i.id === post.id);
      isPost.title = post.title;
      isPost.body = post.body;
    },
    removePost(post) {
      const confirmRemove = confirm("Удалить пост?");
      if (confirmRemove === true) {
        this.posts = this.posts.filter((item) => item.id !== post.id);
      }
    },
    showCreationWindow() {
      this.creationWindow = true;
    },
    hideWindow() {
      this.creationWindow = false;
    },
    changePage(pageNumber) {
      this.page = pageNumber;
      this.fetchPosts();
    },
    async fetchPosts() {
      try {
        const response = await axios.get(
          "https://jsonplaceholder.typicode.com/posts?_limit=10",
          {
            params: {
              _page: this.page,
              _limit: this.limit,
            },
          }
        );
        this.posts = response.data;
        this.totalPages = Math.ceil(this.posts.length / this.limit);
      } catch (e) {
        alert("Error");
      }
    },
  },
  computed: {
    sortedPosts() {
      return [...this.posts].sort((post1, post2) => {
        return post1[this.selectedSort]?.localeCompare(
          post2[this.selectedSort]
        );
      });
    },
  },
};
</script>

<style lang="scss">
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
.btn {
  background-color: mediumaquamarine;
  padding: 0.5em;
  border-radius: 15px;
  max-width: 10em;
  font-size: 25px;
}
.pages-wrapper {
  display: flex;
  gap: 2px;
  justify-content: center;
  margin-top: 2em;
}
.page {
  border: 2px solid;
  padding: 10px;
}
.current-page {
  background-color: lightcyan;
}
.main-terminal{
  display: flex;
  justify-content: space-evenly;
  margin-top: 2em;
}
</style>
