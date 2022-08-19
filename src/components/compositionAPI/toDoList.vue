<template>
  <ul class="toDoList">
    <li
      class="toDoList-item"
      :class="{ isDone: toDo.status }"
      v-for="toDo in toDos"
      :key="toDo.id"
    >
      <toDoItem
        :work="toDo.work"
        :status="toDo.status"
        :description="toDo.description"
        :isModalOpened="toDo.isModalOpened"
        :descriptionButtonName="toDo.descriptionButtonName"
        :isDescriptionOpen="toDo.isDescriptionOpen"
        @deleteToDo="deleteToDo(toDo)"
        @editToDo="editToDo(toDo)"
        @changeStatus="changeStatus($event, toDo)"
        @openDescription="openDescription(toDo)"
      />
      <changeToDoModal
        v-if="toDo.isModalOpened"
        :work="toDo.work"
        :description="toDo.description"
        @saveChanges="saveChanges($event, toDo)"
        @cancelChange="cancelChange(toDo)"
      />
    </li>
  </ul>
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
        work: "Приготовить ужин",
        status: true,
        isModalOpened: false,
        description: "Сварить макароны, пожарить котлеты, сделать салат",
        isDescriptionOpen: false,
        descriptionButtonName: "Развернуть",
      },
      {
        work: "Помыть окна",
        status: false,
        isModalOpened: false,
        description:
          "Купить средство для мытья окон, тряпки для окон лежат в ванной на третьй полке слева",
        isDescriptionOpen: false,
        descriptionButtonName: "Развернуть",
      },
      {
        work: "Поцеловать Тёму",
        status: false,
        isModalOpened: false,
        description: "В носик! Обязательно!!",
        isDescriptionOpen: false,
        descriptionButtonName: "Развернуть",
      },
    ]);

    const deleteToDo = (toDo) => toDos.value.splice(toDos.value.indexOf(toDo), 1);
    const editToDo = (toDo) => (toDo.isModalOpened = true);
    const saveChanges = (newWork, toDo) => {
      toDo.work = newWork[0];
      toDo.description = newWork[1];
      toDos.value = toDos.value.filter((toDo) => toDo.work != "");
      toDo.isModalOpened = false;
    };

    const addToDo = () => {
      let toDo = {
        work: "",
        status: false,
        isModalOpened: true,
        description: "",
        isDescriptionOpen: false,
        descriptionButtonName: "Развернуть",
      };
      toDos.value.push(toDo);
    };

    const cancelChange = (toDo) => {
      toDos.value = toDos.value.filter((toDo) => toDo.work != "");
      toDo.isModalOpened = false;
    };
    const changeStatus = (newStatus, toDo) => (toDo.status = newStatus);

    const openDescription = (toDo) => {
      toDo.isDescriptionOpen = !toDo.isDescriptionOpen;
      if (toDo.descriptionButtonName === "Развернуть") {
        toDo.descriptionButtonName = "Свернуть";
      } else {
        toDo.descriptionButtonName = "Развернуть";
      }
    };

    return {
      toDos,
      deleteToDo,
      editToDo,
      saveChanges,
      addToDo,
      cancelChange,
      changeStatus,
      openDescription,
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