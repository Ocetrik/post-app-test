<template>
  <div class="sorting-wrapper">
    <select v-model="modelValue" @change="changeOption">
      <option disabled value="">Выберите из списка</option>
      <option
        v-for="option in options"
        :key="option.value"
        :value="option.value"
      >
        {{ option.name }}
      </option>
    </select>
  </div>
</template>

<script>
export default {
  props: {
    modelValue: {
      type: String,
    },
    options: {
      type: Array,
      default: () => [],
    },
  },
  methods: {
    changeOption(event) {
      this.$emit("update:modelValue", event.target.value);
    },
  },
};
</script>

<style scoped>
.sorting-wrapper {
  max-width: 1440px;
  display: flex;
  justify-content: right;
  margin-top: 1em;
}
</style>