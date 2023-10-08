<template>
  <div class="app">
    <todo-list
      v-for="(category, categoryName) in categories"
      :key="categoryName"
      :title="categoryName"
      class="category"
    >
      <input
        v-model="newTodoText[categoryName]"
        :placeholder="`Enter a new ${categoryName} task`"
        class="todo-input"
        v-on:keyup.enter="addTodo(categoryName)"
      />
      <button @click="addTodo(categoryName)" class="add-button">
        Add {{ categoryName }}
      </button>
      <draggable :list="category" group="todosapp" class="draggable-list">
        <todo-item
          v-for="todo in category"
          :key="todo.id"
          :item="todo"
          class="todo-item"
          @delete-item="deleteTodo(categoryName, todo.id)"
          @edit-item="editTodo"
        ></todo-item>
      </draggable>
    </todo-list>

    <div>
      <button
        style="
          height: 150px;
          width: 150px;
          border: none;
          border-radius: 10px;
          box-shadow: 12px 12px 2px 1px rgba(132, 126, 126, 0.148);
          font-size: 50px;
        "
        @click="openDialog"
      >
        +
      </button>
    </div>

    <DialogBox
      :is-open="isDialogOpen"
      title="Enter a new list name"
      @close="closeDialog"
    >
      <div class="create-list">
        <input
          v-model="newListName"
          placeholder="Enter a new list name"
          class="list-input"
          v-on:keyup.enter="createList"
        />
        <button @click="createList" class="add-button">Add</button>
        <button @click="closeDialog" class="add-button">Close</button>
      </div>
    </DialogBox>
  </div>
</template>

<script>
import TodoItem from "./components/TodoItem";
import TodoList from "./components/TodoList";
import draggable from "vuedraggable";
import DialogBox from "./components/DialogBox.vue";
export default {
  data() {
    return {
      categories: {
        "Todo List": [
          {
            id: 1,
            todo: "JavaScript",
          },
        ],
        "In Progress ": [],
      },

      newTodoText: {},
      newListName: "",
      isDialogOpen: false,
    };
  },
  components: {
    TodoItem,
    TodoList,
    draggable,
    DialogBox,
  },
  methods: {
    addTodo(categoryName) {
      const text = this.newTodoText[categoryName].trim();

      if (text === "") {
        return;
      }

      const newTodoItem = {
        id: Date.now(),
        todo: text,
      };

      this.categories[categoryName].push(newTodoItem);
      this.newTodoText[categoryName] = "";
    },
    deleteTodo(categoryName, todoId) {
      const category = this.categories[categoryName];
      const index = category.findIndex((todo) => todoId === todo.id);

      if (index !== -1) {
        category.splice(index, 1);
      }
    },
    editTodo({ id, editedTodo }) {
      for (const categoryName in this.categories) {
        const category = this.categories[categoryName];
        const todo = category.find((todo) => todo.id === id);
        if (todo) {
          todo.todo = editedTodo;
          break;
        }
      }
    },
    createList() {
      const newListName = this.newListName.trim();

      if (newListName === "") {
        return;
      }

      this.categories[newListName] = [];
      this.newListName = "";
    },
    openDialog() {
      this.isDialogOpen = true;
    },

    closeDialog() {
      this.isDialogOpen = false;
    },
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
}
.app {
  display: flex;
  align-items: center;
  overflow-x: auto;
  height: 100vh;
}
.category {
  margin: 20px;
  padding: 10px;
  border: none;
  background-color: #f5f5f5;
  border-radius: 10px;
  box-shadow: 12px 12px 2px 1px rgba(132, 126, 126, 0.148);
}

.draggable-list {
  list-style-type: none;
  padding: 0;
}

.todo-item {
  background-color: #f8f5f5;
  border: 1px solid #ddd;
  padding: 10px;
  margin: 5px 0;
  border-radius: 3px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.todo-input {
  width: 200px;
  padding: 5px;
  margin-top: 10px;
  border: 1px solid #ccc;
  border-radius: 3px;
}

.add-button {
  margin-top: 10px;
  padding: 5px 10px;
  margin: 3px;
  background-color: #3cff00;
  color: #fff;
  border: none;
  border-radius: 3px;
  cursor: pointer;
}

.add-button:hover {
  background-color: #def605;
}
.list-input {
  width: 70%;
  padding: 5px;
  border: 1px solid #ccc;
  border-radius: 3px;
  margin-right: 10px;
}

.list-input:focus {
  outline: none;
}
</style>
