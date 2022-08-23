<template>
  <ul class="toDoList">
    <li
      class="toDoList-item"
      :class="{ isDone: toDo.status }"
      v-for="toDo in toDos"
      :key="toDo.id"
    >
      <toDoItem
        :toDo="toDo"
        @deleteToDo="deleteToDo"
        @editToDo="editToDo"
        @changeStatus="changeStatus"
      />
    </li>
  </ul>
  <changeToDoModal
    v-if="selectedTodo"
    :toDo="selectedTodo"
    @saveChanges="saveChanges"
    @cancelChange="cancelChange"
  />
  <button class="toDoList__addButton" @click="addToDo()">Добавить</button>
</template>

<script>
import toDoItem from "./toDoItem.vue";
import changeToDoModal from "./changeToDoModal.vue";
import { ref } from "vue";
export default {
  name: "toDoList",
  components: { changeToDoModal, toDoItem },
  setup() {
    const toDos = ref([
      {
        id: Symbol(),
        work: "Приготовить ужин",
        status: true,
        description: "Сварить макароны, пожарить котлеты, сделать салат",
      },
      {
        id: Symbol(),
        work: "Помыть окна",
        status: false,
        description:
          "Купить средство для мытья окон, тряпки для окон лежат в ванной на третьй полке слева",
      },
      {
        id: Symbol(),
        work: "Поцеловать Тёму",
        status: false,
        description: "В носик! Обязательно!!",
      },
    ]);

    const selectedTodo = ref(null);

    const deleteToDo = (toDo) => {
      toDos.value.splice(toDos.value.indexOf(toDo), 1);
    };
    const editToDo = (todo) => {
      selectedTodo.value = todo;
    };
    const saveChanges = (info) => {
      selectedTodo.value = null;
      if (info.isAdd) {
        toDos.value.push({ ...info.toDo, id: Symbol(), status: false });
      } else { 
        const editTodo = toDos.value.find((x) => x.id === info.toDo.id);
        if (editTodo) {
          editTodo.work = info.toDo.work;
          editTodo.description = info.toDo.description;
        }
      }
    };

    const addToDo = () => {
      selectedTodo.value = {
        id: null,
        work: "",
        status: false,
        description: "",
      };
    };

    const cancelChange = (toDo) => {
      selectedTodo.value = null;
    };
    const changeStatus = (toDo) => {
      toDo.status = !toDo.status;
    };

    return {
      toDos,
      deleteToDo,
      editToDo,
      saveChanges,
      addToDo,
      cancelChange,
      changeStatus,
      selectedTodo,
    };
  },
};
</script>

<style>
.toDoList {
  list-style: none;
  text-align: start;
  padding: 0;
}

.toDoList-item {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  border-bottom: solid 1px #f70000;
  margin-bottom: 1rem;
  color: #f70000;
}

.isDone {
  border-bottom: solid 1px #22a22b;
  color: #22a22b;
}

.toDoList__addButton {
  border-radius: 25px;
  background-color: #22a22b;
  font-weight: bold;
  color: #000000ad;
  font-size: 0.8rem;
  margin: auto;
}
</style>