<template>
  <template v-if="isLoading === false">
    <ul class="to-do-list">
      <li
        class="to-do-list-item"
        :class="{ 'is-done': toDo.status }"
        v-for="toDo in toDos"
        :key="toDo.id"
      >
        <ToDoItem
          :toDo="toDo"
          @deleteToDo="deleteToDo"
          @editToDo="editToDo"
          @changeStatus="changeStatus"
        />
      </li>
    </ul>
    <button class="to-do-list__add-button" @click="addToDo()">Добавить</button>
  </template>

  <div v-else>Загрузка...</div>
  <ChangeToDoModal
    v-if="selectedTodo"
    :toDo="selectedTodo"
    @saveChanges="saveChanges"
    @cancelChange="cancelChange"
  />
</template>

<script>
import ToDoItem from "./ToDoItem.vue";
import ChangeToDoModal from "./ChangeToDoModal.vue";
import axios from "axios";
export default {
  name: "toDoList",
  components: { ChangeToDoModal, ToDoItem },
  data() {
    return {
      toDos: [],
      isModalOpened: false,
      isLoading: false,
      selectedTodo: null,
    };
  },
  async mounted() {
    const res = await axios.get("http://localhost:3000/todos");
    this.isLoading = false;
    this.toDos = res.data;
  },
  methods: {
    async deleteToDo(toDo) {
      this.toDos.splice(this.toDos.indexOf(toDo), 1);
      await axios.delete(`http://localhost:3000/todos/${toDo.id}`);
    },

    editToDo(toDo) {
      this.selectedTodo = toDo;
    },

    async saveChanges(info) {
      this.selectedTodo = null;
      if (info.isAdd) {
        const newToDo = { ...info.toDo, id: Symbol(), status: false };
        this.toDos.push(newToDo);
        await axios.post(`http://localhost:3000/todos`, newToDo);
      } else {
        const editTodo = this.toDos.find((x) => x.id === info.toDo.id);
        if (editTodo) {
          editTodo.work = info.toDo.work;
          editTodo.description = info.toDo.description;
          await axios.put(
            `http://localhost:3000/todos/${editTodo.id}`,
            editTodo
          );
        }
      }
    },

    addToDo() {
      this.selectedTodo = {
        id: null,
        work: "",
        status: false,
        description: "",
      };
    },

    cancelChange() {
      this.selectedTodo = null;
    },

    async changeStatus(toDo) {
      toDo.status = !toDo.status;
      await axios.put(`http://localhost:3000/todos/${toDo.id}`, toDo);
    },
  },
};
</script>

<style>
.to-do-list {
  list-style: none;
  text-align: start;
  padding: 0;
}

.to-do-list-item {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  border-bottom: solid 1px #f70000;
  margin-bottom: 1rem;
  color: #f70000;
}

.is-done {
  border-bottom: solid 1px #22a22b;
  color: #22a22b;
}

.to-do-list__add-button {
  border-radius: 25px;
  background-color: #22a22b;
  font-weight: bold;
  color: #000000ad;
  font-size: 0.8rem;
  margin: auto;
}
</style>