<template>
  <div>
    <drag>
      <div class="overlay">
        <div class="modal">
          <span>Add Menu</span>
          <input type="text" />

          <div>
            <button>Add</button>
          </div>
        </div>
      </div>
    </drag>
  </div>
</template>

<script>
import Drag from "./components/Drag.vue";

export default {
  name: "app",
  components: {
    Drag
  }
};
</script>

<style>
</style>