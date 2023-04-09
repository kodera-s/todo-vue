<script setup lang="ts">
  import { defineProps } from 'vue'
  const props = defineProps({
    buttonSet: {
      type: Number,
      default: 0
    },
    isDisable: {
      type: Boolean,
      default: false
    }
  })

  const displayButton = [
    { buttonText: '追加', buttonColor: 'btn-primary'},
    { buttonText: '作業中', buttonColor: 'btn-warning'},
    { buttonText: '完了', buttonColor: 'btn-success'},
    { buttonText: '削除', buttonColor: 'btn-danger'}
  ]

  const emit = defineEmits(['onClick'])
  const emitClick = () => {
    emit('onClick')
  }  
</script>

<template>
  <button class="btn" :class="displayButton[props.buttonSet].buttonColor" :disabled="isDisable" @click="emitClick">
    {{ displayButton[props.buttonSet].buttonText }}
  </button>
</template>

<style lang="scss" scoped>
</style>