<script setup lang="ts">
  import { ref } from 'vue'

  let inputValue = ref<String>()
  const emit = defineEmits(['inputText'])
  const emitValue = (value) => {
    emit('inputText', inputValue.value)
  }
  const deleteText = () => {
    inputValue.value = ''
  }
  defineExpose({
    deleteText
  })

</script>

<template>
  <input type="text" v-model="inputValue" @blur="emitValue" class="container-fluid">
</template>

<style scoped>

</style>
