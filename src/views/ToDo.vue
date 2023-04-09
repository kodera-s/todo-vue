<script setup lang="ts">
  import { ref, computed } from 'vue'

  import InputText from '../components/atoms/InputText.vue'
  import InputButton from '../components/atoms/Button.vue'

  const isListCheck = ref<Boolean>(false)
  const newItem = ref<String>()
  const todos = ref([])
  const itemID = ref<Number>(0)
  const inputText = ref()
  const displayStatus = ref<String>('all')
  
  const computedList = computed(() => {
    const list = []
    if(displayStatus.value === "inProgress") {
      todos.value.forEach(element => {
        if(!element.isDone) {
          list.push(element)
        }
      })
      return list
    }
    if(displayStatus.value === "finished") {
      todos.value.forEach(element => {
        if(element.isDone) {
          list.push(element)
        }
      })
      return list
    }
    return todos.value
  })

  // inputTextからemitを受け取り
  const emitInputValue = (props) => {
    newItem.value = props
  }

  // リスト追加処理
  const addItem = () => {
    if (!newItem.value) return
    itemID.value ++
    const todo = {
      id: itemID.value,
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

  // status変更
  const changeStatus = (index) => {
    todos.value[index].isDone = !todos.value[index].isDone
  }
</script>

<template>
  <div class="todo">
    <div class="input d-flex align-items-center container-fluid mb-5">
      <div class="input__text col-11 pe-3">
        <InputText ref="inputText" @inputText=emitInputValue />
      </div>
      <InputButton :buttonSet=0 :isDisable="!newItem" @onClick="addItem" />
    </div>
    <div class="container-fluid mb-5">
      <dl class="d-flex container-fluid">
        <dt class="col-1">
          表示
        </dt>
        <dd class="d-flex">
          <div class="me-5">
            <input type="radio" name="display" id="all" value="all" v-model="displayStatus">
            <label for="all">すべて</label>
          </div>
          <div class="me-5">
            <input type="radio" name="display" id="inProgress" value="inProgress" v-model="displayStatus">
            <label for="inProgress">作業中</label>
          </div>
          <div>
            <input type="radio" name="display" id="finished" value="finished" v-model="displayStatus">
            <label for="finished">完了</label>
          </div>
        </dd>
      </dl>

    </div>
    <ul class="todo-list container-fluid">
      <li class="border-bottom d-flex align-items-center pb-3">
        <div class="col-1 ps-3">ID.</div>
        <div class="col-9">Thing</div>
        <div class="col-1 d-flex justify-content-center">Status</div>
        <div class="col-1"></div>
      </li>
      <li v-for="(todo, index) in computedList" :key="index" class="d-flex align-items-start py-2 todo-list__item">
        <div class="col-1 ps-3 pt-2">{{todo.id}}</div>
        <div class="col-9 text-break pe-5 pt-2">{{todo.item}}</div>
        <div class="col-1 d-flex justify-content-center">
          <InputButton :buttonSet="todo.isDone ? 2 : 1" :isDisable="false" @onClick="changeStatus(index)" />
        </div>
        <div class="col-1 d-flex justify-content-center">
          <InputButton :buttonSet=3 :isDisable="false" @onClick="deleteItem(index)" />
        </div>
      </li>
    </ul>
  </div>
</template>

<style lang="scss" scoped>
.todo-list {
  &__item {
    &:hover {
      background-color: #cfe2ff;
    }
  }
}
</style>