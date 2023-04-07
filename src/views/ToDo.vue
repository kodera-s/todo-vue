<script setup lang="ts">
  import { ref } from 'vue'

  import InputText from '../components/atoms/InputText.vue'

  const isListCheck = ref<Boolean>(false)
  const newItem = ref<String>()
  const todos = ref([])
  const inputText = ref()
  
  console.log('todos', todos.value.length)

  // inputTextからemitを受け取り
  const emitInputValue = (props) => {
    newItem.value = props
  }

  // リスト追加処理
  const addItem = () => {
    if (!newItem.value) return
    const todo = {
      checked: false,
      item: newItem.value,
      isDone: false
    }
    // リストに追加
    todos.value.push(todo)
    // 子コンポーネント入力テキスト消去
    inputText.value.deleteText()
    // 子コンポーネントから受け取ったテキストを消去
    newItem.value = ''
  }
  
  // 削除処理
  const deleteItem = (index: number) => {
    todos.value.splice(index, 1)
  }

  const changeCheckAll = () => {
    if (isListCheck.value) {
      console.log('true')
      todos.value.forEach(element =>{
        element.checked = true
      })
    } else {
      console.log('false')
      todos.value.forEach(element =>{
        element.checked = false
      })
    }

  }

</script>

<template>
  <div class="todo">
    <div class="input d-flex container-fluid mb-5">
      <div class="input__text col-11 pe-3">
        <InputText ref="inputText" @inputText=emitInputValue />
      </div>
      <button class="input__button col" @click="addItem">
        Add
      </button>
    </div>
    <ul class="todo-list container-fluid">
      <li class="todo-list__item border-bottom">
        <div class="todo-list__item--checkbox">
          <input type="checkbox" v-model="isListCheck" @change="changeCheckAll" :disabled="todos.length === 0">
        </div>
        <div class="todo-list__item--number">No.</div>
        <div class="todo-list__item--thing">Thing</div>
        <div class="todo-list__item--status">Status</div>
        <div class="todo-list__item--button"></div>
      </li>
      <li v-for="(todo, index) in todos" :key="index" class="todo-list__item py-2">
        <div class="todo-list__item--checkbox">
          <input type="checkbox" v-model="todo.checked">
        </div>
        <div class="todo-list__item--number">{{index + 1}}</div>
        <div class="todo-list__item--thing">{{todo.item}}</div>
        <div class="todo-list__item--status">{{todo.isDone}}</div>
        <div class="todo-list__item--button">
          <button @click="deleteItem(index)">Delete</button>
        </div>
      </li>
    </ul>
  </div>
</template>

<style lang="scss" scoped>
.todo-list {
  &__item {
    display: flex;
    &--checkbox {
      width: 5%;
      text-align: center;
    }
    &--number {
      width: 10%;
    }
    &--thing {
      width: 70%;
    }
    &--status {
      width: 10%;
    }
    &--button {
      width: 5%;
    }
    &--title {
      color: #f00;
    }
  }
}

</style>