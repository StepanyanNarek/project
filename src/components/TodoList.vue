<template>
  <div class="todo-list">
    <div class="title">
      {{ title }}
    </div>
    <slot></slot>
  </div>
</template>
<script>
export default {
  props: ["title"],
  data() {
    return {};
  },
};
</script>
<style>
.todo-list {
  padding: 10px 20px;
  margin: 0 10px;
  border: 1px solid rgb(153, 143, 143);
  font-family: sans-serif;
  width: 350px;
}
.title {
  padding: 10px;
  font-size: 20px;
  font-weight: 600;
  margin-bottom: 10px;
  text-align: center;
  border-bottom: 1px solid red;
}
</style>
