<template>
  <div v-if="show" class="post-create-window">
    <div @click.stop class="post-create-window__content">
      <slot></slot>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    show: {
      type: Boolean,
      default: false,
    },
  },
};
</script>

<style scoped>
.post-create-window {
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  background-color: rgba(0, 0, 0, 0.5);
  position: fixed;
  display: flex;
}
.post-create-window__content {
  margin: auto;
  background: white;
  padding: 2em;
  border-radius: 15px;
  width: 50%;
}
</style>