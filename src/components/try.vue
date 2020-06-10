<template>
  <div>
    <draggable
      class="row pt-4 mx-30"
      :cards="cards"
      :disabled="!enabled"
      ghost-class="ghost"
      :move="checkMove"
      @start="dragging = true"
      @end="dragging = false"
    >
      <div class="col-sm-3 text-center mb-400" v-for="(card, index) in cards" :key="index">
        <div>
          <h5 @click="editCard(card, index)">
            <i class="fa fa-pencil-square-o" aria-hidden="true">{{ card.headline }}</i>
          </h5>
        </div>

        <draggable
          :cards="cards"
          :disabled="!enabled"
          ghost-class="ghost"
          :move="checkMove"
          @start="dragging = true"
          @end="dragging = false"
        >
          <div class="card-title text-center" v-for="(link, index) in card.links" :key="index">
            <h6>
              <i class="fa fa-pencil-square-o" aria-hidden="true">{{ link.name }}</i>
            </h6>
          </div>
        </draggable>
      </div>
    </draggable>
    <div class="col-3">
      <div class="form-group">
        <div>
          <button type="button" class="btn btn-dark" @click="openDialogBox">Add Card</button>

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
                        <button
                          type="button"
                          class="btn btn-primary"
                          @click="addHeadline"
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
                              <button
                                type="button"
                                class="btn btn-secondary"
                                data-dismiss="modal"
                              >Close</button>

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
                              >
                                <span aria-hidden="true">&times;</span>
                              </button>
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
                              <button
                                type="button"
                                class="btn btn-secondary"
                                data-dismiss="modal"
                              >Close</button>

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
      </div>
    </div>
  </div>
</template>

<script>
import draggable from "vuedraggable";
import $ from "jquery";
export default {
  name: "Linkitem",
  components: {
    draggable
  },
  data() {
    return {
      enabled: true,
      name: "",
      checked: false,
      url: "",
      headline: "",
      headlineIndex: null,
      editing: false,
      editData: {},
      cards: [
        {
          headline: "Social",
          links: [
            {
              name: "Facebook",
              url: "https://"
            },
            {
              name: "Twitter",
              url: "https://"
            },
            {
              name: "Instagram",
              url: "https://"
            }
          ]
        },
        {
          headline: "Information",
          links: [
            {
              name: "ContactNo",
              url: "https://"
            },
            {
              name: "Email_Id",
              url: "https://"
            },
            {
              name: "CVR_No",
              url: "https://"
            }
          ]
        },
        {
          headline: "Account",
          links: [
            {
              name: "Login",
              url: "https://"
            },

            {
              name: "Register",
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
      console.log(data);

      this.editData = data;
      $("#edit").modal("show");
    },

    editCardSave() {
      console.log("works edit save");

      console.log(this.cards[this.editIndex]);

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

    editlink() {}
  }
};
</script>

<style >
</style>