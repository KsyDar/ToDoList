<template>
  <div class="to-do__information">
    <input
      type="checkbox"
      class="to-do-status"
      :checked="toDo.status"
      @change="changeStatus"
    />
    {{ toDo.work }}
    <p class="to-do__description" :class="{ 'to-do__description--open': isDescriptionOpen }">
      {{ toDo.description }}
    </p>
  </div>
  <div class="to-do__buttons">
    <button
      class="to-do__button to-do__button--description"
      @click="openDescription"
    >
      {{ descriptionButtonName }}
    </button>
    <button class="to-do__button to-do__button--edit" @click="editToDo">
      Изменить
    </button>
    <button class="to-do__button" @click="deleteToDo">Удалить</button>
  </div>
</template>

<script>
import { ref } from "@vue/reactivity";
export default {
  name: "ToDoItem",
  emits: ["deleteToDo", "editToDo", "changeStatus"],
  props: {
    toDo: Object,
  },
  setup(props, { emit }) {
    const deleteToDo = () => emit("deleteToDo", props.toDo);
    const editToDo = () => emit("editToDo", props.toDo);
    const changeStatus = () => emit("changeStatus", props.toDo);

    const isDescriptionOpen = ref(false);
    const descriptionButtonName = ref("Развернуть");

    const openDescription = () => {
      isDescriptionOpen.value = !isDescriptionOpen.value;
      descriptionButtonName.value =
        isDescriptionOpen.value === true ? "Свернуть" : "Развернуть";
    };

    return {
      deleteToDo,
      editToDo,
      changeStatus,
      openDescription,
      isDescriptionOpen,
      descriptionButtonName
    };
  },
};
</script>

<style>
.to-do__information {
  cursor: pointer;
}

.to-do__description {
  display: none;
  color: #000;
}

.to-do__description--open {
  display: table-row;
}

.to-do__buttons {
  margin-left: 10rem;
  font-size: 0.8rem;
  display: flex;
}

.to-do__button {
  margin-left: 1rem;
  margin-bottom: 0.5rem;
  border-radius: 25px;
  background-color: #f70000;
  font-weight: bold;
  color: #000000ad;
}

.to-do__button:hover {
  border: solid 1px #000;
}

.to-do__button--edit {
  background-color: #f3f700;
}

.to-do__button--description {
  background-color: #d0ef99;
}
</style>