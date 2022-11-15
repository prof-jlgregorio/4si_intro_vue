<script setup>
import { reactive, ref, computed, watch } from 'vue';

  const appName = "Todo List"
  const author = "Jorge Luís Gregório"

  const currentId = ref(3);

  const todos = reactive([
    {id: 1, name :"Estudar estrutura de dados", status : "done"},
    {id: 2, name :"Estudar Javascript", status : "doing"},
    {id: 3, name :"Aprender Vue.js", status : "todo"},
  ])

  const currentTodo = ref("")

  const generateId = () => {
    currentId.value++;
    return currentId.value;
  }

  const addTodo = () => {
    let id = generateId();
    todos.push( { id: id, name: currentTodo.value, status : "todo" } );
    currentTodo.value = "";
  }

  const deleteTodo = (id) => {
    let index = todos.findIndex(todo => todo.id === id);
    console.log(index);
    todos.splice(index, 1);
  }

  const total = reactive(
    {todo: 0, doing: 0, done: 0 }
  );

  const totals = computed( () => {
    total.todo = 0;
    total.doing = 0;
    total.done = 0;
    todos.forEach(el => {
      if(el.status === "todo")
        total.todo++;
      else if (el.status === "doing")
        total.doing++;
      else 
        total.done++
    });
  })


</script>

<template>
  <div>
    <h1>{{ appName }}</h1>
    <h2>{{ author }}</h2>
    <hr>
    <input type="text" name="" id="" v-model="currentTodo">
    <button @click="addTodo()">Adicionar</button>

    <ul>
      <li v-for="todo in todos" :class="todo.status == 'todo' ? 'todo' : todo.status == 'doing' ? 'doing' : 'done' " > 
        {{ todo.name }}

        <select name="status" id="status" v-model="todo.status">
          <option value="todo">A fazer</option>
          <option value="doing">Fazendo</option>
          <option value="done">Feito</option>
        </select>

        <button @click="deleteTodo(todo.id)">Excluir</button>

      </li>

    </ul>

    <h3>Resumo</h3>
    <ul>
      <li>A fazer: {{ total.todo }}</li>
      <li>Fazendo: {{ total.doing }}</li>
      <li>Feito: {{ total.done }}</li>
    </ul>

    <h2></h2>
  </div>
</template>

<style scoped>

.todo{
  color: blue;
}

.done{
  text-decoration: line-through;
  color: green;
}

.doing{
  color: green;
}

</style>
