<template>
  <div class="app">
    <div
      class="drop-zone"
      @drop="onDrop($event, 1)"
      @dragover.prevent
      @dragenter.prevent
    >
      <div
        v-for="item in listOne"
        :key="item.title"
        class="drag-el"
        draggable
        @dragstart="startDrag($event, item)"
      >
        {{ item.title }}
      </div>
    </div>
    <div class="drop-zone">
      <div v-for="item in listTwo" :key="item.title" class="drag-el">
        {{ item.title }}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Footer",
  components: {},
  data() {
    return {
      items: [
        { id: 1, title: "CVR No", list: 1 },
        { id: 2, title: "Email", list: 2 },
        { id: 3, title: "Phone", list: 1 },
        { id: 4, title: "Card-Details", list: 1 },
      ],
    };
  },
  computed: {
    listOne() {
      return this.items.filter((item) => item.list === 1);
    },
    listTwo() {
      return this.items.filter((item) => item.list === 2);
    },
  },

  methods: {
    startDrag: (evt, item) => {
      evt.dataTransfer.dropEffect = "move";
      evt.dataTransfer.effectAllowed = "move";
      evt.dataTransfer.setData("itemID", item.id);
      console.log(item);
    },

    onDrop: (evt, list) => {
      const itemID = evt.dataTransfer.getData("itemID");
      const item = this.items.find((item) => item.id == itemID);
      console.log(item);

      item.list = list;
      console.log(evt, list);
    },
  },
};
</script>

<style>
.drop-zone {
  background-color: #eee;
  margin-bottom: 10px;
  padding: 10px;
}

.drag-el {
  background-color: #fff;
  margin-bottom: 10px;
  padding: 5px;
}
</style>
