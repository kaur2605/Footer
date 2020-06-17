<template>
  <div class="row">
    <div class="col-12">
      <div class="menu_btn">
        <button class="btn btn-secondary mx-2 my-2 w-10" @click="sort">Original List</button>
        <button type="button" class="btn btn-dark mx-2 my-2 w-10" @click="openDialogBox">Add Link</button>
        <div class="modal fade" id="add" tabindex="-1" role="dialog" aria-hidden="true">
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
                    <input
                      v-if="!editing"
                      type="text"
                      class="form-control"
                      id="menu"
                      v-model="menu"
                      placeholder="Enter Menu name"
                    />
                    <input
                      v-else
                      type="text"
                      class="form-control"
                      id="menu"
                      v-model="edata.menu"
                      placeholder="Enter Menu name"
                    />
                  </div>
                  <div class="form-group">
                    <label for="url">enter url</label>
                    <input
                      v-if="!editing"
                      type="text"
                      class="form-control"
                      id="url"
                      v-model="url"
                      placeholder="enter url"
                    />
                    <input
                      v-else
                      type="text"
                      class="form-control"
                      id="url"
                      v-model="edata.url"
                      placeholder="enter url"
                    />
                  </div>
                  <div class="form-check">
                    <input type="checkbox" class="form-check-input" id="check" />
                    <label class="form-check-label" for="check">open in new Window</label>
                  </div>
                </form>
              </div>
              <div class="modal-footer">
                <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
                <button
                  v-if="editing"
                  type="button"
                  class="btn btn-primary"
                  @click="EditMenu"
                >Save edit changes</button>

                <button v-else type="button" class="btn btn-primary" @click="addlink">Save changes</button>
              </div>
            </div>
          </div>
        </div>

        <!--addtext-->
        <button
          type="button"
          class="btn btn-info m-4"
          data-toggle="modal"
          data-target="#text"
        >Add Text</button>
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
      </div>
      <div class="menu">
        <draggable
          class="list-group"
          tag="ul"
          v-model="list"
          v-bind="dragOptions"
          @start="drag = true"
          @end="drag = false"
        >
          <transition-group type="transition" :list="!drag ? 'flip-list' : null">
            <li class="list-group-item" v-for="(element,index) in list" :key="index">
              <span class="span_name">
                <i
                  :class="          element.fixed ? 'fa fa-anchor' : 'glyphicon glyphicon-pushpin' "
                  @click="element.fixed = !element.fixed"
                  aria-hidden="true"
                ></i>
                {{ element.menu}}
              </span>
              <div class="pencil">
                <button type="button" class="btn btn-info">
                  <i
                    class="fa fa-pencil-square-o"
                    aria-hidden="true"
                    @click="EditData(element, index)"
                  >&nbsp;&nbsp;</i>
                </button>
                &nbsp;&nbsp;
                <button
                  type="button"
                  class="btn btn-danger"
                  @click="DeleteList1(index)"
                >
                  <i class="fa fa-trash trash" aria-hidden="true"></i>
                </button>
              </div>
            </li>
          </transition-group>
        </draggable>
        <draggable
          class="list-group"
          tag="ul"
          v-model="list2"
          v-bind="dragOptions"
          @start="drag = true"
          @end="drag = false"
        >
          <transition-group type="transition" :list2="!drag ? 'flip-list' : null">
            <li class="list-group-item" v-for="(element,index) in list2" :key="index">
              <span class="span_name">
                <i
                  :class="          element.fixed ? 'fa fa-anchor' : 'glyphicon glyphicon-pushpin' "
                  @click="element.fixed = !element.fixed"
                  aria-hidden="true"
                ></i>
                {{ element.menu}}
              </span>
              <div class="pencil">
                <button type="button" class="btn btn-info">
                  <i
                    class="fa fa-pencil-square-o"
                    aria-hidden="true"
                    @click="EditData(element, index)"
                  >&nbsp;&nbsp;</i>
                </button>
                &nbsp;&nbsp;
                <button
                  type="button"
                  class="btn btn-danger"
                  @click="DeleteList2(index)"
                >
                  <i class="fa fa-trash trash" aria-hidden="true"></i>
                </button>
              </div>
            </li>
          </transition-group>
        </draggable>
        <draggable
          class="list-group"
          tag="ul"
          v-model="list3"
          v-bind="dragOptions"
          @start="drag = true"
          @end="drag = false"
        >
          <transition-group type="transition" :list3="!drag ? 'flip-list' : null">
            <li class="list-group-item" v-for="(element,index) in list3" :key="index">
              <span class="span_name">
                <i
                  :class="          element.fixed ? 'fa fa-anchor' : 'glyphicon glyphicon-pushpin' "
                  @click="element.fixed = !element.fixed"
                  aria-hidden="true"
                ></i>
                {{ element.menu}}
              </span>
              <div class="pencil">
                <button type="button" class="btn btn-info">
                  <i
                    class="fa fa-pencil-square-o"
                    aria-hidden="true"
                    @click="EditData(element, index)"
                  >&nbsp;&nbsp;</i>
                </button>
                &nbsp;&nbsp;
                <button
                  type="button"
                  class="btn btn-danger"
                  @click="DeleteList3(index)"
                >
                  <i class="fa fa-trash trash" aria-hidden="true"></i>
                </button>
              </div>
            </li>
          </transition-group>
        </draggable>
      </div>
    </div>
  </div>
</template>

<script>
import draggable from "vuedraggable";
import $ from "jquery";

export default {
  name: "Transition",
  display: "Transitions",
  order: 7,
  components: {
    draggable
  },
  data() {
    return {
      title: "Footer Content",
      FooterText: "Share some details",
      menu: "",
      url: "",
      checked: false,
      editing: false,
      edata: {},
      list2: [
        {
          menu: "Cateogy",
          url: "https://",
          checked: false
        },
        {
          menu: "Information",
          url: "https://",
          checked: false
        },
        {
          menu: "Privacy",
          url: "https://",
          checked: false
        }
      ],

      list: [
        {
          menu: "Headline",
          url: "https://",
          checked: false
        },
        {
          menu: "About",
          url: "https://",
          checked: false
        },
        {
          menu: "Home",
          url: "https://",
          checked: false
        }
      ],
      list3: [
        {
          menu: "Return",
          url: "https://",
          checked: false
        },
        {
          menu: "refund",
          url: "https://",
          checked: false
        },
        {
          menu: "Privacy",
          url: "https://",
          checked: false
        }
      ],

      drag: false
    };
  },
  methods: {
    sort() {
      this.list = this.list.sort((a, b) => a.order - b.order);
    },
    openDialogBox() {
      this.editing = false;
      this.menu = "";
      this.url = "";
      this.checked = "";
      $("#add").modal("show");
    },
    opentextbox() {
      $("#text").modal("show");
    },
    openeditbox() {
      $("#edit").modal("show");
    },

    textsave() {
      $("#text").modal("hide");
    },
    addlink() {
      this.editing = false;
      this.list.push({
        menu: this.menu,
        url: this.url,
        checked: this.checked
      });

      console.log(this.list);

      (this.menu = ""), (this.url = ""), (this.checked = false);
      $("#add").modal("hide");
    },

    EditData(data) {
      this.edata = data;
      this.editing = true;
      $("#add").modal("show");
    },

    DeleteList1(index) {
      this.list.splice(index, 1);
    },

    DeleteList2(index) {
      this.list2.splice(index, 1);
    },
    DeleteList3(index) {
      this.list3.splice(index, 1);
    },

    EditMenu() {
      $("#add").modal("hide");

      /*let menuindex = this.list.findIndex(
        element => element.menu === this.editData.menu
      );

      this.editing = false;
      this.list[menuindex] = this.editData;*/

      this.menu = "";
      this.url = "";
      this.checked = "";
    }
  },
  computed: {
    dragOptions() {
      return {
        animation: 200,
        group: "description",
        disabled: false,
        ghostClass: "ghost"
      };
    }
  }
};
</script>

<style>
.menu {
  display: flex;
  flex-direction: row;
  margin: 30px;
  margin-right: 70px;
}
.button {
  margin-top: 35px;
}
.flip-list-move {
  transition: transform 1.5s;
}
.no-move {
  transition: transform 0s;
}
.ghost {
  opacity: 0.5;
  background: #c8ebfb;
}
.list-group {
  display: flex;
  flex-direction: row;
  min-height: 20px;
  min-width: 50px;
}

.heading {
  border: 1px solid;
  background-color: rgb(199, 196, 196);
  position: relative;
}
.list-group-item {
  cursor: move;
  width: 500px;
  margin-top: 10px;
  margin-right: 50px;
  border: 1px solid;
  background-color: rgb(199, 196, 196);
  display: flex;
  justify-content: space-between;
}
.pencil {
  padding-left: 170px;
  margin-left: 50px;
  float: right;

  margin-top: 0;
}

.span_name {
  align-self: center;
}

.list-group-item i {
  cursor: pointer;
}
</style>