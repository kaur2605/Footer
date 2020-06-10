<template>
  <div>
    <div class="form-group">
      <!-- Button trigger modal -->
      <button type="button" class="btn btn-dark mx-2 my-2 w-10" @click="openDialogBox">Add Link</button>
      <!--Add menu-->
      <!-- Modal -->
      <div
        class="modal fade"
        id="add"
        tabindex="-1"
        role="dialog"
        aria-labelledby="exampleModalScrollableTitle"
        aria-hidden="true"
      >
        <div class="modal-dialog modal-dialog-scrollable" role="document">
          <div class="modal-content">
            <div class="modal-header">
              <h5 class="modal-title" id="add">Add Menu</h5>
              <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                <span aria-hidden="true">&times;</span>
              </button>
            </div>
            <div class="modal-body">
              <form>
                <div class="form-group">
                  <label for="menu">Enter Menu Name</label>
                  <input type="text" class="form-control" id="menu" placeholder="Enter Menu name" />
                </div>
                <div class="form-group">
                  <label for="url">enter url</label>
                  <input type="text" class="form-control" id="url" v-mode placeholder="enter url" />
                </div>
                <div class="form-check">
                  <input type="checkbox" class="form-check-input" id="check" />
                  <label class="form-check-label" for="check">open in new Window</label>
                </div>
              </form>
            </div>
            <div class="modal-footer">
              <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
              <button type="button" class="btn btn-primary" @click="addlink">Save changes</button>
            </div>
          </div>
        </div>
      </div>

      <!--add text-->

      <button
        type="button"
        class="btn btn-info m-4"
        data-toggle="modal"
        data-target="#text"
      >Add Text</button>
    </div>
    <!--  Text area Modal -->
    <div class="modal fade" id="text" tabindex="-1" role="dialog" aria-hidden="true">
      <div class="modal-dialog" role="document">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="Text">Footer Content</h5>
            <button type="button" class="close" data-dismiss="modal" aria-label="Close">
              <span aria-hidden="true">&times;</span>
            </button>
          </div>
          <div class="modal-body">
            <label for="title">Enter Title</label>
            <input type="text" v-model="title" />
            <textarea id="text" class="w-100" v-model="FooterText"></textarea>
          </div>
          <div class="modal-footer">
            <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
            <button type="button" class="btn btn-primary" @click="textsave">Save changes</button>
          </div>
        </div>
      </div>
    </div>

    <div class="row">
      <div class="col-3 border border-secondary ml-3">
        <h2>{{title}}</h2>
        <h5>{{FooterText}}</h5>
      </div>

      <div ref="draggableContainer" id="draggable-container">
        <div
          id="draggable-header"
          @mousedown="dragMouseDown"
          v-for="(item, index) in list"
          :key="index"
          class="bg-info p-2 border border-dark w-100"
        >
          <h2>
            <i class="fa fa-arrows" aria-hidden="true">&nbsp;&nbsp; {{ item.menu }}</i>
            <div class="pencil">
              <i class="fa fa-pencil-square-o" aria-hidden="true">&nbsp;&nbsp;</i>&nbsp;&nbsp;
              <i class="fa fa-trash trash" aria-hidden="true"></i>
            </div>
          </h2>
        </div>
        <h6></h6>
      </div>
    </div>
  </div>
</template>

<script>
import $ from "jquery";
export default {
  name: "Dragable",
  data: function() {
    return {
      positions: {
        clientX: undefined,
        clientY: undefined,
        movementX: 0,
        movementY: 0
      },
      title: "footer",
      FooterText: "Share some details",
      enabled: true,
      list: [
        {
          menu: "Headline",
          url: "https://",
          checked: false
        }
      ]
    };
  },
  methods: {
    openDialogBox() {
      $("#add").modal("show");
    },
    opentextbox() {
      $("#text").modal("show");
    },

    textsave() {
      $("#text").modal("hide");
    },

    addlink() {
      this.list.push({
        menu: this.menu,
        url: this.url,
        checked: this.checked
      });
      (this.menu = ""), (this.url = ""), (this.checked = false);
      $("#add").modal("hide");
    },

    checkMove: function(e) {
      window.console.log("Future index: " + e.draggedContext.futureIndex);
    },

    editCard(data) {
      console.log("here is data" + data);

      this.editData = data;
      $("#edit").modal("show");
    },

    editCardSave() {
      console.log("works edit save");

      console.log("here is card" + this.cards[this.editIndex]);

      let cardIndex = this.cards.findIndex(
        card => card.headline === this.editData.headline
      );

      this.editing = false;
      this.cards[cardIndex] = this.editData;

      this.headline = "";
      this.links.name = "";
      this.links.url = "";
      this.checked = "";
      $("#add").modal("hide");
    },

    dragMouseDown: function(event) {
      event.preventDefault();
      // get the mouse cursor position at startup:
      this.positions.clientX = event.clientX;
      this.positions.clientY = event.clientY;
      document.onmousemove = this.elementDrag;
      document.onmouseup = this.closeDragElement;
    },
    elementDrag: function(event) {
      event.preventDefault();
      this.positions.movementX = this.positions.clientX - event.clientX;
      this.positions.movementY = this.positions.clientY - event.clientY;
      this.positions.clientX = event.clientX;
      this.positions.clientY = event.clientY;
      // set the element's new position:
      this.$refs.draggableContainer.style.top =
        this.$refs.draggableContainer.offsetTop -
        this.positions.movementY +
        "px";
      this.$refs.draggableContainer.style.left =
        this.$refs.draggableContainer.offsetLeft -
        this.positions.movementX +
        "px";
    },
    closeDragElement() {
      document.onmouseup = null;
      document.onmousemove = null;
    }
  }
};
</script>

<style>
#draggable-container {
  position: absolute;
  z-index: 9;
}
#draggable-header {
  z-index: 10;
}
.pencil {
  padding-left: 170px;
  margin-left: 50px;
  margin-right: 0;
  float: right;
  margin-top: 0;
}
</style>