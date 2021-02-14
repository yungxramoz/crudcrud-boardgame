<template>
  <v-data-table
    :headers="headers"
    :items="boardgames"
    sort-by="name"
    class="elevation-2"
    style="width: 100vw"
  >
    <template v-slot:top>
      <v-toolbar flat>
        <v-toolbar-title>Board Games</v-toolbar-title>
        <v-divider class="mx-4" inset vertical></v-divider>
        <v-spacer></v-spacer>
        <v-dialog v-model="dialog" max-width="500px">
          <template v-slot:activator="{ on, attrs }">
            <v-btn
              icon
              color="primary"
              dark
              class="mb-2"
              v-bind="attrs"
              v-on="on"
            >
              <v-icon large>mdi-plus</v-icon>
            </v-btn>
          </template>
          <v-card>
            <v-card-title>
              <span class="headline">Add Boardgame</span>
            </v-card-title>
            <v-card-text>
              <v-container>
                <v-row>
                  <v-col cols="12">
                    <v-text-field
                      filled
                      hide-details
                      v-model="editedItem.name"
                      label="Name"
                      prepend-icon="mdi-checkerboard"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12">
                    <v-text-field
                      filled
                      hide-details
                      v-model="editedItem.publisher"
                      label="Publisher"
                      prepend-icon="mdi-domain"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="6">
                    <v-text-field
                      filled
                      hide-details
                      v-model="editedItem.minPlayer"
                      label="Min Player"
                      prepend-icon="mdi-account-multiple"
                      type="number"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="6">
                    <v-text-field
                      filled
                      hide-details
                      v-model="editedItem.maxPlayer"
                      label="Max Player"
                      type="number"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="6">
                    <v-text-field
                      filled
                      hide-details
                      v-model="editedItem.minDuration"
                      label="Min Duration"
                      prepend-icon="mdi-clock"
                      type="number"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="6">
                    <v-text-field
                      filled
                      hide-details
                      v-model="editedItem.maxDuration"
                      label="Max Duration"
                      type="number"
                    ></v-text-field>
                  </v-col>
                </v-row>
              </v-container>
            </v-card-text>

            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="blue darken-1" text @click="close">
                Cancel
              </v-btn>
              <v-btn color="success darken-1" text @click="save">
                Save
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>
        <v-dialog v-model="dialogDelete" max-width="500px">
          <v-card>
            <v-card-title class="headline">
              Delete Action
            </v-card-title>
            <v-card-text>
              Are you sure you want to delete this item?
              <ul>
                <li>
                  <strong>
                    {{ deletedItem.name }}
                    ({{ deletedItem.publisher }})
                  </strong>
                </li>
              </ul>
            </v-card-text>
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="blue darken-1" text @click="closeDelete">
                Cancel
              </v-btn>
              <v-btn color="red darken-1" text @click="deleteItemConfirm">
                Delete
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>
      </v-toolbar>
    </template>
    <template v-slot:[`item.actions`]="{ item }">
      <v-btn color="blue darken-1" icon @click="editItem(item)">
        <v-icon>
          mdi-pencil
        </v-icon>
      </v-btn>
      <v-btn color="red darken-1" icon @click="deleteItem(item)">
        <v-icon>
          mdi-delete
        </v-icon>
      </v-btn>
    </template>
  </v-data-table>
</template>

<script>
export default {
  name: "CrudTable",

  props: {
    apiUrl: String,
  },

  data: () => ({
    dialog: false,
    dialogDelete: false,
    deletedItem: {},
    editAction: "",
    headers: [
      { text: "Name", value: "name", width: "4%" },
      { text: "Publisher", value: "publisher", width: "4%" },
      { text: "Min Player", value: "minPlayer", width: "3%" },
      { text: "Max Player", value: "maxPlayer", width: "3%" },
      { text: "Min Duration", value: "minDuration", width: "3%" },
      { text: "Max Duration", value: "maxDuration", width: "3%" },
      { text: "Actions", value: "actions", width: "2%", sortable: false },
    ],
    boardgames: [],
    defaultItem: {
      _id: "",
      name: "",
      publisher: "",
      minPlayer: null,
      maxPlayer: null,
      minDuration: null,
      maxDuration: null,
    },
    editedItem: {},
  }),

  watch: {
    dialog(val) {
      val || this.close();
    },
    dialogDelete(val) {
      val || this.closeDelete();
    },

    apiUrl() {
      this.initialize();
    },
  },

  created() {
    this.editedItem = Object.assign({}, this.defaultItem);
    this.initialize();
  },

  methods: {
    initialize() {
      this.$axios.get(this.apiUrl + "/boardgame").then((response) => {
        console.log(response);
        this.boardgames = response.data;
      });
    },

    editItem(item) {
      this.editedItem = Object.assign({}, item);
      this.dialog = true;
    },

    deleteItem(item) {
      this.deletedItem = Object.assign({}, item);
      this.dialogDelete = true;
    },

    deleteItemConfirm() {
      this.$axios
        .delete(this.apiUrl + "/boardgame/" + this.deletedItem._id)
        .then(() => {
          this.boardgames.splice(
            this.boardgames.findIndex(
              (item) => item._id === this.deletedItem._id
            ),
            1
          );
          this.closeDelete();
        });
    },

    close() {
      this.dialog = false;
      this.$nextTick(() => {
        this.editedItem = Object.assign({}, this.defaultItem);
      });
    },

    closeDelete() {
      this.dialogDelete = false;
      this.$nextTick(() => {
        this.editedItem = Object.assign({}, this.defaultItem);
      });
    },

    save() {
      const sendItem = {
        name: this.editedItem.name,
        minPlayer: this.editedItem.minPlayer,
        maxPlayer: this.editedItem.maxPlayer,
        minDuration: this.editedItem.minDuration,
        maxDuration: this.editedItem.maxDuration,
        publisher: this.editedItem.publisher,
      };

      //add new item params
      let id = "";
      let action = "post";
      let callBack = (response) => {
        this.boardgames.push(response.data);
      };

      if (this.editedItem._id !== "") {
        //edit existing items params
        id = `/${this.editedItem._id}`;
        action = "put";
        callBack = () => {
          const index = this.boardgames.findIndex(
            (item) => item._id === this.editedItem._id
          );
          Object.assign(this.boardgames[index], this.editedItem);
        };
      }

      this.$axios({
        method: action,
        url: this.apiUrl + "/boardgame" + id,
        data: sendItem,
      }).then((response) => {
        console.log(response);
        callBack(response);
        this.close();
      });
    },
  },
};
</script>
