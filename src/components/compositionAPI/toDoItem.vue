<template>
  <div class="toDo-information">
    <input
      type="checkbox"
      class="toDo-status"
      :checked="toDo.status"
      @change="changeStatus"
    />
    {{ toDo.work }}
    <p class="toDo-description" :class="{ descriptionOpen: isDescriptionOpen }">
      {{ toDo.description }}
    </p>
  </div>
  <div class="toDo__buttons">
    <button
      class="toDo__button toDo__button--description"
      @click="openDescription"
    >
      {{ descriptionButtonName }}
    </button>
    <button class="toDo__button toDo__button--edit" @click="editToDo">
      Изменить
    </button>
    <button class="toDo__button" @click="deleteToDo">Удалить</button>
  </div>
</template>

<script>
import { ref } from "@vue/reactivity";
export default {
  name: "toDoItem",
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
.toDo-information {
  cursor: pointer;
}

.toDo-description {
  display: none;
  color: #000;
}

.descriptionOpen {
  display: table-row;
}

.toDo__buttons {
  margin-left: 10rem;
  font-size: 0.8rem;
  display: flex;
}

.toDo__button {
  margin-left: 1rem;
  margin-bottom: 0.5rem;
  border-radius: 25px;
  background-color: #f70000;
  font-weight: bold;
  color: #000000ad;
}

.toDo__button:hover {
  border: solid 1px #000;
}

.toDo__button--edit {
  background-color: #f3f700;
}

.toDo__button--description {
  background-color: #d0ef99;
}
</style>