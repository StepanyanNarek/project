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
        :placeholder="`Enter a new ${categoryName.toLowerCase()} todo`"
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
    <div class="create-list">
      <input
        v-model="newListName"
        placeholder="Enter list name"
        class="list-input"
        v-on:keyup.enter="createList"
      />
    </div>
  </div>
</template>

<script>
import TodoItem from "./components/TodoItem";
import TodoList from "./components/TodoList";
import draggable from "vuedraggable";

export default {
  data() {
    return {
      categories: {
        "Todo List": [
          {
            id: 1,
            todo: "CSS",
          },
        ],
        "In Progress": [
          {
            id: 2,
            todo: "JavaScript",
          },
        ],
        Completed: [
          {
            id: 3,
            todo: "Vue.js",
          },
        ],
      },
      newTodoText: {},
      newListName: "",
    };
  },
  components: {
    TodoItem,
    TodoList,
    draggable,
  },
  methods: {
    addTodo(categoryName) {
      const text = this.newTodoText[categoryName].trim();

      if (text === "") {
        return; // Не добавляем пустые задачи
      }

      const newTodoItem = {
        id: Date.now(), // Генерируем уникальный ID
        todo: text,
      };

      this.categories[categoryName].push(newTodoItem); // Добавляем в соответствующий массив
      this.newTodoText[categoryName] = ""; // Очищаем поле ввода
    },
    deleteTodo(categoryName, todoId) {
      const category = this.categories[categoryName];
      const index = category.findIndex((todo) => todo.id === todoId);

      if (index !== -1) {
        category.splice(index, 1); // Удаляем задачу из массива
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
        return; // Не добавляем пустые названия списка
      }

      // Создаем новый пустой список с заданным именем
      this.categories[newListName] = [];
      this.newListName = ""; // Очищаем поле ввода
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
  overflow-x: scroll;
  height: 100vh;
  background: linear-gradient(
    50deg,
    rgb(10, 0, 0),
    rgb(165, 63, 88),
    #00bfff,
    #008080,
    red,
    yellow,
    #71b995,
    rgb(216, 207, 207),
    rgb(97, 73, 3)
  );
  background-size: 100%;
  animation: gradientBG 1s linear infinite;
}

@keyframes gradientBG {
  0% {
    background-position: 0% 50%;
  }
  50% {
    background-position: 100% 50%;
  }
  100% {
    background-position: 0% 50%;
  }
}
.category {
  margin: 20px;
  padding: 10px;
  border: 1px solid #ccc;
  background-color: #f5f5f5;
  border-radius: 10px;
  box-shadow: 12px 12px 2px 1px rgba(9, 0, 0, 0.148);
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
  padding: 5px;
  margin-top: 10px;
  border: 1px solid #ccc;
  border-radius: 3px;
}

.add-button {
  margin-top: 10px;
  padding: 5px 10px;
  background-color: #3cff00;
  color: #fff;
  border: none;
  border-radius: 3px;
  cursor: pointer;
}

.add-button:hover {
  background-color: #0056b3;
}
.list-input {
  width: 70%;
  padding: 5px;
  border: 1px solid #ccc;
  border-radius: 3px;
  margin-right: 10px;
}

.list-input:focus {
  outline: none; /* Убираем рамку фокуса */
}
</style>
