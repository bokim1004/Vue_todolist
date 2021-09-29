<template>
  <h1>To Do App</h1>
  <!-- v-on:submit.prevent는 기본적인 동작(새로고침)을 하지않도록 제어하는 이벤트 -->
  <!-- form안에 버튼이 있어서 클릭하면 form의 submit이 먹힌다 -->
  <form @submit.prevent="addTodo()">
    <label> New ToDo </label>

    <!-- form요소를 개발할 때 v-model을 사용한다. 양방향 데이터바인딩으로 내용이
    작성되면 해당 내용은 선언한 데이터 값에 바로 적용된다. -->
    <input v-model="newTodo" name="newTodo" autocomplete="off" />
    <button>Add ToDo</button>
  </form>
  <h2>To Do List</h2>
  <ul>
    <li v-for="(todo, index) in todos" :key="index">
      <span :class="{ done: todo.done }" @click="doneTodo(todo)">
        {{ todo.content }}
      </span>
      <button @click="removeTodo(index)">Remove</button>
    </li>
  </ul>
  <h4 v-if="todos.length === 0">Empty list</h4>
</template>

<script>
import { ref } from "vue";

export default {
  name: "App",
  setup() {
    const newTodo = ref("");
    const defaultData = [{ done: false, content: "write a blog post" }];
    const todosData = JSON.parse(localStorage.getItem("todos")) || defaultData;
    // ref는 자식 엘리먼트에 접근하는 기능
    const todos = ref(todosData);
    function addTodo() {
      if (newTodo.value) {
        todos.value.push({ done: false, content: newTodo.value });
        //새로입력한 값
        newTodo.value = "";
      }
      saveData();
    }
    function doneTodo(todo) {
      todo.done = !todo.done;
      saveData();
    }

    function removeTodo(index) {
      //index 한개 요소 제거
      todos.value.splice(index, 1);
      saveData();
    }
    function saveData() {
      const storageData = JSON.stringify(todos.value);
      localStorage.setItem("todos", storageData);
    }
    return {
      todos,
      newTodo,
      addTodo,
      doneTodo,
      removeTodo,
      saveData,
    };
  },
};
</script>

<style lang="scss">
$border: 2px solid
  rgba(
    $color: white,
    $alpha: 0.35,
  );
$size1: 6px;
$size2: 12px;
$size3: 18px;
$size4: 24px;
$size5: 48px;
$backgroundColor: #27292d;
$textColor: white;
$primaryColor: #a0a4d9;
$secondTextColor: #1f2023;
body {
  margin: 0;
  padding: 0;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  background-color: $backgroundColor;
  color: $textColor;
  #app {
    max-width: 600px;
    margin-left: auto;
    margin-right: auto;
    padding: 20px;
    h1 {
      font-weight: bold;
      font-size: 28px;
      text-align: center;
    }
    form {
      display: flex;
      flex-direction: column;
      width: 100%;
      label {
        font-size: 14px;
        font-weight: bold;
      }
      input,
      button {
        height: $size5;
        box-shadow: none;
        outline: none;
        padding-left: $size2;
        padding-right: $size2;
        border-radius: $size1;
        font-size: 18px;
        margin-top: $size1;
        margin-bottom: $size2;
      }
      input {
        background-color: transparent;
        border: $border;
        color: inherit;
      }
    }
    button {
      cursor: pointer;
      background-color: $primaryColor;
      border: 1px solid $primaryColor;
      color: $secondTextColor;
      font-weight: bold;
      outline: none;
      border-radius: $size1;
    }
    h2 {
      font-size: 22px;
      border-bottom: $border;
      padding-bottom: $size1;
    }
    ul {
      padding: 10px;
      li {
        display: flex;
        justify-content: space-between;
        align-items: center;
        border: $border;
        padding: $size2 $size4;
        border-radius: $size1;
        margin-bottom: $size2;
        span {
          cursor: pointer;
        }
        .done {
          text-decoration: line-through;
        }
        button {
          font-size: $size2;
          padding: $size1;
        }
      }
    }
    h4 {
      text-align: center;
      opacity: 0.5;
      margin: 0;
    }
  }
}
</style>
