<template>
  <div class="modal-wrapper">
    <div class="modal">
      <label class="modal__label">
        Название:
        <input type="text" class="modal__input" v-model="work" />
      </label>
      <label class="modal__label">
        Описание:
        <textarea
          cols="30"
          rows="10"
          class="modal__input modal__input--description"
          v-model="description"
        ></textarea>
      </label>
      <div class="modal__buttons">
        <button
          class="modal__buttons-item modal__buttons-item--save"
          @click="saveChanges()"
        >
          Сохранить
        </button>
        <button class="modal__buttons-item" @click="cancelChange()">
          Отмена
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "changeToDoModal",
  emits: ["saveChanges", "cancelChange"],
  props: {
    work: String,
    description: String,
  },

  methods: {
    saveChanges() {
      this.$emit("saveChanges", [this.work, this.description]);
    },
    cancelChange() {
      this.$emit("cancelChange");
    },
  },
};
</script>

<style>
.modal-wrapper {
  height: 100%;
  width: 100%;
  position: absolute;
  top: 0;
  left: 0;
  background-color: #04040491;
}

.modal {
  background-color: #fff;
  width: 25rem;
  height: 25rem;
  margin: auto;
  margin-top: 15rem;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  border-radius: 5px;
  color: #000;
  border: solid 2px;
}

.modal__label {
  margin: auto;
  margin-left: 0.8rem;
}

.modal__input {
  padding: 0;
  width: 23rem;
  height: 2rem;
  border-radius: 5px;
}

.modal__input--description {
  height: 12rem;
  resize: none;
  border: inset 2px #8d7c7cf5;
}

.modal__buttons {
  display: flex;
  justify-content: space-between;
  width: 100%;
}

.modal__buttons-item {
  margin: 1rem;
  margin-top: 2rem;
  background-color: #f70000;
  border-radius: 25px;
  font-weight: bold;
  color: #000000ad;
}
.modal__buttons-item--save {
  background-color: #22a22b;
}
</style>