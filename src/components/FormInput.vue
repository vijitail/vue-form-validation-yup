<template>
  <div :class="[ 'form-group', !!error && 'has-error' ]">
    <label class="form-label" for="email">{{label}}</label>
    <input
      :id="name"
      :name="name"
      :type="type"
      :value="value"
      @input="$emit('input', $event.target.value)"
      class="form-input"
      @blur="$emit('validate')"
      @keypress="$emit('validate')"
    />
    <p class="form-input-hint" v-if="!!error">{{ error }}</p>
  </div>
</template>

<script>
export default {
  name: "form-input",
  props: {
    type: { required: true },
    label: { required: true },
    name: { required: true },
    value: { required: true },
    error: { required: true }
  }
};
</script>