<template>
  <div class="todo-item">
    <div v-if="!isEditing">{{ item.todo }}</div>
    <div v-else>
      <input v-model="editedTodo" @keyup.enter="saveEdit" @blur="saveEdit" />
    </div>
    <button @click="toggleEdit" class="edit-button">
      {{ isEditing ? "Save" : "Edit" }}
    </button>
    <button class="delete-button" @click="deleteItem">Delete</button>
  </div>
</template>

<script>
export default {
  props: ["item"],
  data() {
    return {
      isEditing: false,
      editedTodo: this.item.todo,
    };
  },
  methods: {
    deleteItem() {
      this.$emit("delete-item", this.item.id);
    },
    toggleEdit() {
      this.isEditing = !this.isEditing;
    },
    saveEdit() {
      if (this.isEditing) {
        this.$emit("edit-item", {
          id: this.item.id,
          editedTodo: this.editedTodo,
        });
        this.isEditing = false;
      }
    },
  },
};
</script>

<style>
.todo-item {
  
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 10px 20px;
  border: 1px solid #ccc;
  border-radius: 5px;
  margin-bottom: 10px;
  
}

.todo-text {
  flex-grow: 1;
}

.edit-button {
  background-color: #007bff; /* Цвет фона */
  color: white; /* Цвет текста */
  border: none; /* Убираем границу */
  padding: 5px 10px; /* Отступы вокруг текста */
  border-radius: 5px; /* Закругляем углы */
  cursor: pointer; /* Курсор при наведении */

  /* Дополнительные стили при наведении */
  transition: background-color 0.3s ease-in-out;
}

.edit-button:hover {
  background-color: #0056b3; /* Изменяем цвет фона при наведении */
}

/* Стили для редактирования задачи */
.todo-item input {
  flex: 1;
  border: 1px solid #ccc; /* Граница для поля ввода */
  border-radius: 5px; /* Закругляем углы */
  padding: 5px; /* Отступы вокруг текста */
}

/* Дополнительные стили для редактирования задачи при активности */
.todo-item input:focus {
  outline: none; /* Убираем обводку */
  border-color: #007bff; /* Изменяем цвет границы при активности */
}
.delete-button {
  background-color: #ff0000;
  color: #ffffff;
  border: none;
  padding: 5px 10px;
  border-radius: 3px;
  cursor: pointer;
}

.delete-button:hover {
  background-color: #cc0000;
}
</style>
