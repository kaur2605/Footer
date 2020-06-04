<template>
  <footer class="page-footer font-small bg-secondary pt-4">
    <!-- Footer Links -->
    <div class="container-fluid text-center text-md-left bg-secondary">
      <!-- Grid row -->
      <div class="row h-100">
        <!-- Grid column -->
        <div class="col-md-2 mt-md-0 mt-3">
          <!-- Content -->
          <h5 class="text-uppercase">Footer Content</h5>
          <p>some text</p>
        </div>
        <div class="col-md-6 h-100">
          <draggable
            :list="list"
            :disabled="!enabled"
            class="list-group d-flex"
            ghost-class="ghost"
            :move="checkMove"
            @start="dragging = true"
            @end="dragging = false"
          >
            <transition-group class="listitem">
              <div class="list card" v-for="(element, index) in list" :key="index">
                <h3
                  data-toggle="modal"
                  data-target="#add"
                  @click="editMenu(element, index)"
                  class="card-header"
                >
                  <i class="fa fa-pencil-square-o" aria-hidden="true">{{ element.menu }}</i>
                </h3>
              </div>
            </transition-group>
          </draggable>

          <draggable
            :listitems="listitems"
            :disabled="!enabled"
            class="list-group d-flex"
            ghost-class="ghost"
            :move="checkMove"
            @start="dragging = true"
            @end="dragging = false"
          >
            <transition-group class="listitems mx-200">
              <div v-for="(item, index) in listitems" :key="index">
                <ul data-toggle="modal" data-target="#add">
                  <li>
                    <i class="fa fa-pencil-square-o" aria-hidden="true">{{ item.link }}</i>
                  </li>
                </ul>
              </div>
            </transition-group>
          </draggable>
        </div>
        <!-- Grid column -->
        <div class="col-4">
          <div class="form-group">
            <div class="col-4">
              <button type="button" class="btn btn-primary" @click="openDialogBox">Add Menu</button>

              <div
                class="modal fade"
                id="add"
                tabindex="-1"
                role="dialog"
                aria-labelledby="add"
                aria-hidden="true"
              >
                <div class="modal-dialog">
                  <div class="modal-content">
                    <div class="modal-header">
                      <h5 class="modal-title" id="add">Add Menu Tab</h5>
                      <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                        <span aria-hidden="true">&times;</span>
                      </button>
                    </div>
                    <div class="modal-body">
                      <form>
                        <div class="form-group">
                          <label for="menu">Enter Menu Name</label>
                          <input
                            type="text"
                            v-model="menu"
                            class="form-control"
                            id="menu"
                            placeholder="enter menu name"
                          />
                        </div>
                        <div class="form-group">
                          <label for="url">Enter Url name</label>
                          <input
                            v-model="url"
                            type="text"
                            class="form-control"
                            id="url"
                            placeholder="enter url name"
                          />
                        </div>
                        <div class="form-check">
                          <input
                            type="checkbox"
                            class="form-check-input"
                            id="tabcheck"
                            v-model="checked"
                          />
                          <label class="form-check-label" for="checked">open in new tab</label>
                        </div>
                      </form>
                    </div>
                    <div class="modal-footer">
                      <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
                      <button
                        v-if="editing"
                        type="button"
                        class="btn btn-primary"
                        @click="editMenuSave"
                      >Save edit changes</button>
                      <button v-else type="button" class="btn btn-primary" @click="add">Save changes</button>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
      <!-- Grid row -->
    </div>
    <!-- Footer Links -->

    <!-- Copyright -->
    <div class="footer-copyright text-center py-3 bg-dark text-white">
      © 2020 Copyright:
      <a class="text-white" href="https://Meeshop.com/">Meeshop.com</a>
    </div>
    <!-- Copyright -->
  </footer>
</template>

<script>
import draggable from "vuedraggable";
import $ from "jquery";
export default {
  name: "Drag",
  order: 0,
  components: {
    draggable
  },
  data() {
    return {
      enabled: true,
      menu: "",
      url: "",
      list: [
        { menu: "Info", url: "/test", checked: false },
        { menu: "Account", url: "/", checked: false },
        { menu: "Social", url: "/", checked: false }
      ],
      listitems: [
        { link: "facebook", url: "" },
        { link: "twitter", url: "" },
        { link: "instagram", url: "" }
      ],
      dragging: false,
      checked: false,
      editIndex: null,
      editing: false
    };
  },
  computed: {
    draggingInfo() {
      return this.dragging ? "under drag" : "";
    }
  },
  methods: {
    openDialogBox() {
      $("#add").modal("show");
    },

    add() {
      this.list.push({
        menu: this.menu,
        url: this.url,
        checked: this.checked,
        editing: true
      });
      this.menu = "";
      this.url = "";
      this.checked = false;
      $("#add").modal("hide");
    },

    checkMove: function(e) {
      window.console.log("Future index: " + e.draggedContext.futureIndex);
    },

    editMenu(data, index) {
      this.menu = data.menu;
      this.url = data.url;
      this.checked = data.checked;
      this.editing = true;
      this.edit = data;
      this.editIndex = index;

      this.menu = "";
      this.url = "";
      this.checked = "";
    },

    editMenuSave() {
      console.log("works edit save");

      console.log(this.list[this.editIndex]);

      this.editing = false;
      this.list[this.editIndex].menu = this.menu;
      this.list[this.editIndex].url = this.url;
      this.list[this.editIndex].checked = this.checked;

      this.menu = "";
      this.url = "";
      this.checked = "";
      $("#add").modal("hide");
    }
  }
};
</script>
<style scoped>
.ghost {
  opacity: 0.5;
  background: #c8ebfb;
}

.row {
  width: 100%;
  height: 200px;
  background: #c8ebfb;
  padding: 30px;
  margin-top: 30%;
}

.form-group {
  display: flexbox;
  flex-direction: column;
}

.listitem {
  display: flex;
  flex-direction: row;
}

.list {
  display: flex;
  margin: 30px;
  padding: 10px;
}

.form-check .button {
  margin-left: 100%;
}
</style>
