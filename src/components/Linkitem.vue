<template>
  <div class="border border-secondary p-2 mx-5 my-4 bg-secondary">
    <div class="form-group">
      <button type="button" class="btn btn-dark mx-2 my-2 w-10" @click="openDialogBox">Add Link</button>

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

      <!-- add new model below-->
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
              <h5 class="modal-title" id="add">Add Headline</h5>
              <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                <span aria-hidden="true">&times;</span>
              </button>
            </div>
            <div class="modal-body">
              <form>
                <div class="form-group">
                  <label for="headline">Enter Headline Name</label>
                  <input
                    type="text"
                    v-model="headline"
                    class="form-control"
                    id="headline"
                    placeholder="Enter headline"
                  />
                </div>
                <div class="form-group">
                  <button
                    type="button"
                    class="btn btn-primary"
                    data-toggle="collapse"
                    data-target="#link"
                    aria-controls="link"
                  >Edit links</button>
                  <div class="modal-footer">
                    <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
                    <button type="button" class="btn btn-primary" @click="addHeadline">Save Headline</button>
                  </div>

                  <div class="collapse" id="link" aria-labelledby="link" aria-hidden="true">
                    <div class="modal-dialog">
                      <div class="modal-content">
                        <div class="modal-header">
                          <h5 class="modal-title" id="link">Add Menu Tab</h5>
                          <button
                            type="button"
                            class="close"
                            data-dismiss="modal"
                            aria-label="Close"
                          >
                            <span aria-hidden="true">&times;</span>
                          </button>
                        </div>
                        <div class="modal-body">
                          <form>
                            <div class="form-group">
                              <label for="menu">Enter Menu Name</label>
                              <input
                                type="text"
                                v-model="name"
                                class="form-control"
                                id="name"
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
                            type="button"
                            class="btn btn-primary"
                            data-toggle="modal"
                            data-dismiss="modal"
                            @click="addlink"
                          >Save Links</button>
                        </div>
                      </div>
                    </div>
                  </div>
                </div>
              </form>
            </div>
          </div>
        </div>
      </div>
      <!-- edit model below-->
      <div
        class="modal fade"
        id="edit"
        tabindex="-1"
        role="dialog"
        aria-labelledby="edit"
        aria-hidden="true"
      >
        <div class="modal-dialog">
          <div class="modal-content">
            <div class="modal-header">
              <h5 class="modal-title" id="edit">Edit Headline</h5>
              <button type="button" class="close" data-dismiss="modal" aria-label="Close">Close</button>
            </div>
            <div class="modal-body">
              <form>
                <div class="form-group">
                  <label for="headline">Enter Headline Name</label>
                  <input
                    type="text"
                    v-model="editData.headline"
                    class="form-control"
                    id="headline"
                    placeholder="Enter headline"
                  />
                </div>
                <div class="form-group">
                  <button
                    type="button"
                    class="btn btn-primary"
                    data-toggle="collapse"
                    data-target="#link"
                    aria-controls="link"
                  >Edit links</button>
                  <div class="modal-footer">
                    <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
                    <button
                      v-if="editing"
                      type="button"
                      class="btn btn-primary"
                      @click="editCardSave"
                    >Save edit changes</button>
                    <button
                      v-else
                      type="button"
                      class="btn btn-primary"
                      @click="addHeadline"
                      data-dismiss="modal"
                    >Save Headline</button>
                  </div>

                  <div class="collapse" id="link" aria-labelledby="link" aria-hidden="true">
                    <div class="modal-dialog">
                      <div class="modal-content">
                        <div class="modal-header">
                          <h5 class="modal-title" id="link">Add Menu Tab</h5>
                          <button
                            type="button"
                            class="close"
                            data-dismiss="modal"
                            aria-label="Close"
                          >close</button>
                        </div>
                        <div class="modal-body">
                          <form>
                            <div v-for="link in editData.links" :key="link.name">
                              <div class="form-group">
                                <label for="menu">Enter Menu Name</label>
                                <input
                                  type="text"
                                  v-model="link.name"
                                  class="form-control"
                                  id="name"
                                  placeholder="enter menu name"
                                />
                              </div>
                              <div class="form-group">
                                <label for="url">Enter Url name</label>
                                <input
                                  v-model="link.url"
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
                            </div>
                          </form>
                        </div>
                        <div class="modal-footer">
                          <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>

                          <button
                            type="button"
                            class="btn btn-primary"
                            data-toggle="modal"
                            data-dismiss="modal"
                            @click="addlink"
                          >Save Links</button>
                        </div>
                      </div>
                    </div>
                  </div>
                </div>
              </form>
            </div>
          </div>
        </div>
      </div>
    </div>
    <div class="row">
      <div class="col-3 border border-secondary ml-3">
        <h2>{{title}}</h2>
        <h5>{{FooterText}}</h5>
      </div>

      <div v-for="(card, index) in cards" :key="index" class="bg-info p-2 border border-dark">
        <div ref="draggableContainer" id="draggable-container">
          <button
            @click="editCard(card, index)"
            class="btn"
            id="draggable-header"
            @mousedown="dragMouseDown"
          >
            <h5 class="text-left p-6">
              <i class="fa fa-arrows" aria-hidden="true">&nbsp;&nbsp; {{ card.headline }}</i>
              <div class="pencil">
                <i class="fa fa-pencil-square-o" aria-hidden="true">&nbsp;&nbsp;</i>&nbsp;&nbsp;
                <i class="fa fa-trash trash" aria-hidden="true"></i>
              </div>
            </h5>
          </button>
        </div>

        <!--         <draggable
            :cards="cards"
            :disabled="!enabled"
            ghost-class="ghost"
            :move="checkMove"
            @start="dragging = true"
            @end="dragging = false"
          >
            <div class="card-title text-center" v-for="(link, index) in card.links" :key="index">
              <h6>{{link.name}}</h6>
            </div>
        </draggable>-->
      </div>
    </div>
  </div>
</template>

<script>
import $ from "jquery";
export default {
  name: "Linkitem",
  components: {},
  data() {
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
      name: "",
      checked: false,
      url: "",
      headline: "",
      headlineIndex: null,
      editing: true,
      editData: {},
      cards: [
        {
          headline: "Headline",
          links: [
            {
              name: "",
              url: "https://"
            }
          ]
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
    remove(index) {
      console.log("removed");

      this.cards.$remove(index);
    },

    openlinkbox() {
      $("#link").modal("show");
    },
    addHeadline() {
      let indexNumber = this.cards.push({
        headline: this.headline,
        links: []
      });
      this.headline = "";
      this.headlineIndex = indexNumber - 1;
      $("#add").modal("hide");
    },

    addlink() {
      console.log(this.cards[this.headlineIndex]);
      this.cards[this.headlineIndex].links.push({
        name: this.name,
        url: this.url,
        checked: this.checked
      });

      this.name = "";
      this.url = "";
      this.checked = false;
      $("#link").modal("hide");
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

      console.log($(".draggableContainer").offset());

      this.$refs.draggableContainer.style.top =
        this.$refs.draggableContainer.offset.Top -
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

<style >
.pencil {
  padding-left: 170px;
  margin-left: 50px;
  margin-right: 0;
  float: right;
  margin-top: 0;
}
</style>