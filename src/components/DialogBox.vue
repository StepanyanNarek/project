<template>
  <div class="dialog-overlay" v-if="isOpen">
    <div class="dialog">
      <h3>{{title}}</h3>
      <slot></slot>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    isOpen: Boolean,
    title: String,
  },
  methods: {
    closeDialog() {
      this.$emit("close");
    },
  },
};
</script>

<style>
.dialog-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(199, 191, 191, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
}

.dialog  {
  height: 100px;
  width: 600px;
  background: white;
  padding: 20px;
  border-radius: 5px;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.2);
  
}
</style>
