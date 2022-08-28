<template>
  <h1>Список дел</h1>
  <ToDoList />
</template>

<script>
import ToDoList from "./components/optionsAPI/ToDoList.vue";
export default {
  components: { ToDoList },
};
</script>

<style>
#app {
  border: double 4px #213547;
  padding: 1rem;
  display: flex;
  flex-direction: column;
  align-items: stretch;
}

h1 {
  text-align: center;
  font-size: 2rem;
  text-decoration: underline;
}
</style>
