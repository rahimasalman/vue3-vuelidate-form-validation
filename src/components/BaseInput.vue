<template>
  <div class="input-wrap">
    <label v-if="label">{{ label }}</label>
    <input
      type="text"
      :value="modelValue"
      @input="$emit('update:modelValue', $event.target.value)"
      v-bind="$attrs"
    />
  </div>
</template>

<script setup>
defineProps({
  label: {
    type: [String, Boolean],
    default: false,
  },
  modelValue: {
    type: String,
    default: "",
  },
});
</script>

<style lang="scss" scoped>
.input-wrap {
  display: flex;
  flex-direction: column;

  input {
    padding: 8px 12px;
    font-size: 16px;
    color: #000000;
  }
  input:focus {
    background-color: #f5f8f5;
    border-radius: 5px;
  }
}
</style>
