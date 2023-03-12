<template>
  <div class="input-group">
    <label class="input-group-text">{{ spanValue }}</label>
    <select
        class="form-select"
        v-bind:value="data"
        @input="inputData"
    >
      <option selected>{{ initial_value }}</option>
      <option v-for="value in options" :key="value.id">{{ value.value }}</option>
    </select>
  </div>
</template>

<script>
export default {
  name: "input-simple-select",
  props: [
      "options",
      "initial_value",
      "spanValue"
  ],
  data() {
    return{
      data: this.initial_value
    }
  },
  methods: {
    inputData(event){
      this.data = event.target.value;
      this.$emit('getData', this.data);
    }
  },
  mounted() {
    this.$emit('getData', this.data);
  }
}
</script>

<style scoped>

</style>