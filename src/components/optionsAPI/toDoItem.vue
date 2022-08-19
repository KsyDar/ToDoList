<template>
  <div class="toDo-information">
    <input
      type="checkbox"
      class="toDo-status"
      v-model="status"
      @change="changeStatus()"
    />
    {{ work }}
    <p class="toDo-description" :class="{ descriptionOpen: isDescriptionOpen }">
      {{ description }}
    </p>
  </div>
  <div class="toDo__buttons">
    <button
      class="toDo__button toDo__button--description"
      @click="openDescription()"
    >
      {{ descriptionButtonName }}
    </button>
    <button class="toDo__button toDo__button--edit" @click="editToDo()">
      Изменить
    </button>
    <button class="toDo__button" @click="deleteToDo()">Удалить</button>
  </div>
</template>

<script>
export default {
  name: "toDoItem",
  props: {
    work: String,
    status: Boolean,
    isModalOpened: Boolean,
    description: String,
    descriptionButtonName: String,
    isDescriptionOpen: Boolean,
  },
  methods: {
    deleteToDo() {
      this.$emit("deleteToDo");
    },

    editToDo() {
      this.$emit("editToDo");
    },
    changeStatus() {
      this.$emit("changeStatus", this.status);
    },
    openDescription() {
      this.$emit("openDescription");
    },
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