<template>
  <div class="form-input">
    <label>{{ label }}</label>
    <input :type="type" :value="modelValue" @input="onInput" class="input-field" :placeholder="placeholder" />
    <p v-if="error" class="error">{{ error }}</p>
  </div>
</template>

<script>
export default {
  props: {
    label: String,
    modelValue: String,
    error: String,
    placeholder: String,
    type: {
      type: String,
      default: 'text'
    }
  },
  methods: {
    onInput(event) {
      this.$emit('update:modelValue', event.target.value);
      this.$emit('validate');
    }
  }
}
</script>

<style>
.form-input {
  margin-bottom: 15px;
  display: flex;
  flex-direction: column;
}

.input-field {
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 4px;
  font-size: 14px;
  width: 100%;
  transition: border-color 0.3s ease;
}

.input-field:focus {
  border-color: #4CAF50;
  outline: none;
}

.error {
  color: red;
  font-size: 12px;
  margin-top: 5px;
}
</style>
