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
  background-color: #00ffb3; 
  color: white; 
  border: none; 
  padding: 5px 10px;
  border-radius: 5px; 
  cursor: pointer; 

  transition: background-color 0.3s ease-in-out;
}

.edit-button:hover {
  background-color: #0056b3;
}

.todo-item input {
  flex: 1;
  border: 1px solid #ccc; 
  border-radius: 5px; 
  padding: 5px; 
}

.todo-item input:focus {
  outline: none; 
  border-color: #007bff; 
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
