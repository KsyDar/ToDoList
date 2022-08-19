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
export default {
  name: "toDoList",
  components: { changeToDoModal, toDoItem },
  data() {
    return {
      toDos: [
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
      ]      
    }
  },
  
  methods: {
    deleteToDo(toDo) {
      this.toDos.splice(this.toDos.indexOf(toDo), 1);
    },

    editToDo(toDo) {
      (toDo.isModalOpened = true)
    },

    saveChanges(newWork, toDo) {
      toDo.work = newWork[0];
      toDo.description = newWork[1];
      this.toDos = this.toDos.filter((toDo) => toDo.work != "");
      toDo.isModalOpened = false;
    },

    addToDo() {
      let toDo = {
        work: "",
        status: false,
        isModalOpened: true,
        description: "",
        isDescriptionOpen: false,
        descriptionButtonName: "Развернуть",
      };
      this.toDos.push(toDo);
    },

    cancelChange(toDo) {
      this.toDos = this.toDos.filter((toDo) => toDo.work != "");
      toDo.isModalOpened = false;
    },
    
    changeStatus(newStatus, toDo) {
      toDo.status = newStatus
    },

    openDescription(toDo) {
      toDo.isDescriptionOpen = !toDo.isDescriptionOpen;
      if (toDo.descriptionButtonName === "Развернуть") {
        toDo.descriptionButtonName = "Свернуть";
      } else {
        toDo.descriptionButtonName = "Развернуть";
      }
    }
  }
}
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