<template>
  <div class="create-post">
    <form @submit.prevent>
      <input
        class="create-post__input"
        placeholder="Название статьи"
        type="text"
        v-model="post.title"
      />
      <input
        class="create-post__input"
        placeholder="Описание статьи"
        type="text"
        v-model="post.body"
      />
      <button :disabled="isInvalidPost" class="create-post__btn" @click="addPost">Загрузить</button>
    </form>
  </div>
</template>

<script>
export default {
  data() {
    return {
      post: {
        title: "",
        body: "",
      },
    };
  },
  methods: {
    addPost() {
      this.$emit("add", {...this.post, id: Date.now()});
      this.post = {
        title: "",
        body: "",
        
      };
    },
  },
  computed: {
    isInvalidPost() {
      return this.post.title === "" || this.post.body === "" 
    }
  }
};
</script>

<style scoped>
.create-post__input {
  width: 80%;
  border: 2px solid #cccccc;
  padding: 1em;
}
.create-post__btn {
  background-color: mediumaquamarine;
  padding: 0.7em;
  border-radius: 15px;
  max-width: 10em;
  font-size: 20px;
  margin-top: 1em;
}
form {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 15px;
}
</style>