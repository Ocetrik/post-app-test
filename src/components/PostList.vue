<template>
  <div class="post-list">
    <div class="post-list__title">Список постов</div>
    <transition-group name="post-anim">
      <div v-for="post in posts" :key="post.id" class="post">
        <div class="post-item">
          <div class="post-item__title">
            Статья {{ post.id }}: {{ post.title }}
          </div>
          <div class="post-item__body">
            Текст статьи {{ post.id }}: {{ post.body }}
          </div>
        </div>
        <div class="post-btns">
          <button class="delete-btn btn" @click="$emit('remove', post)">
            Удалить
          </button>
          <button
            @click="showEditWindow(post.id)"
            class="post-list__edit-btn btn"
          >
            Редактировать
          </button>
        </div>
      </div>
    </transition-group>
    <div
      @click="hideEditWindow"
      v-if="modalEdit.show"
      :editIdPost="modalEdit.postId"
      class="post-edit"
    >
      <div @click.stop class="post-edit__content">
        <div class="post-edit__input">
          <input type="text" v-model="post.title" />
          <input type="text" v-model="post.body" />
        </div>
        <button @click="editPost" class="post-edit__btn btn">Сохранить</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      modalEdit: {
        show: false,
        postId: null,
      },
      post: { title: "", body: "", id: null },
    };
  },
  props: {
    posts: {
      type: Array,
      required: true,
    },
  },
  created() {
  },
  methods: {
    editPost() {
      this.id = Date.now();
      this.$emit("edit", this.post);
      this.post = {
        title: "",
        body: "",
      };
      this.modalEdit.show = false
    },
    showEditWindow(postId) {
      const postValue = this.posts.find((i) => i.id === postId);
      this.post = JSON.parse(JSON.stringify(postValue))
      this.modalEdit.postId = postId;
      this.modalEdit.show = true;
    },
    hideEditWindow() {
      this.modalEdit.show = false;
    },
  },
};
</script>

<style scoped>
.post {
  border: 3px solid #cccccc;
  margin: 0 auto;
  margin-top: 2em;
  max-width: 60%;
  padding: 1em;
  display: flex;
  justify-content: space-between;
}
.post-list__title {
display: flex;
justify-content: center;
}
.post-list {
  font-size: 25px;
  gap: 15px;
}
.post-btns {
  display: flex;
  flex-direction: column;
}
.btn {
  background-color: mediumaquamarine;
  padding: 0.5em;
  border-radius: 15px;
  max-width: 10em;
  font-size: 15px;
  margin-top: 1em;
}
.post-anim-item {
  display: inline-block;
  margin-right: 10px;
}
.post-anim-enter-active,
.post-anim-leave-active {
  transition: all 0.5s ease;
}
.post-anim-enter-from,
.post-anim-leave-to {
  opacity: 0;
  transform: translateY(30px);
}

.post-edit {
  top: 0;
  right: 0;
  left: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, 0.5);
  position: fixed;
  display: flex;
}
.post-edit__content {
  margin: auto;
  background: white;
  padding: 2em;
  border-radius: 15px;
  width: 50%;
}
.post-edit__input {
  display: flex;
  flex-direction: column;
  gap: 15px;
}
input {
  height: 2em;
}
.post-edit__btn {
  margin-top: 1em;
}
</style>
