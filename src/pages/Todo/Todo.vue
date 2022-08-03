<template>
  <div>
    <v-toolbar flat>
      <v-card-title class="title">
        CRUD PROJECT
      </v-card-title>
      <v-spacer></v-spacer>
      <v-flex xs12 sm6 md3>
        <v-text-field
          align-center
          v-model="search"
          clearable
          append-icon="search"
          label="Search"
          single-line
          hide-details
        ></v-text-field>
      </v-flex>
      <dialogCus
        :editedItem="editedItem"
        :dialog="dialog"
        :formTitle="formTitle"
        @save="save"
        @close="close"
      />
    </v-toolbar>
    <tableTodo
      @editItem="editItem"
      @deleteItem="deleteItem"
      :headers="headers"
      :search="search"
      :projects="projects"
    />
  </div>
</template>

<script>
import { HTTP } from "../../api/api";
import dialogCus from "./DialogCus.vue";
import tableTodo from "./TableTodo.vue";
export default {
  data: () => ({
    dialog: false,
    search: "",
    headers: [
      { text: "Chủ đầu tư", align: "left", sortable: true, value: "investor" },
      { text: "Dựa án", value: "project" },
      { text: "Số vốn (tỷ)", value: "investment" },
      { text: "Thời hạn (tháng)", value: "time" },
      { text: "Hành động", value: "name", sortable: false }
    ],
    projects: [],
    errors: [],
    editedIndex: -1,
    editedItem: {
      investor: "",
      project: "",
      investment: "",
      time: ""
    },
    defaultItem: {
      investor: "",
      project: "",
      investment: "",
      time: ""
    }
  }),
  created() {
    this.loadProjects();
  },
  mounted() {},
  watch: {
    dialog(val) {
      val || this.close();
    }
  },
  components: {
    dialogCus,
    tableTodo
  },
  computed: {
    formTitle() {
      return this.editedIndex === -1 ? "New Item" : "Edit Item";
    }
  },
  methods: {
    editItem(item) {
      console.log(item);
      this.editedItem = { ...item.item };
      this.editedIndex = 1;
      this.dialog = true;
    },

    deleteItem(item) {
      if (confirm("Are you sure you want to delete this item?") === true) {
        HTTP.delete(`projects/${item}`).then(() => this.loadProjects());
      } else {
        alert("ABORTED Delete Projects");
      }
    },
    close() {
      this.dialog = false;
      this.loadProjects;
      setTimeout(() => {
        this.editedIndex = -1;
      }, 300);
      this.loadForm();
    },

    save() {
      if (this.editedIndex === -1) {
        this.saveProjects();
      } else {
        HTTP.patch(`projects/${this.editedItem.id}`, this.editedItem)
          .then(res => {
            this.loadProjects();
          })
          .catch(e => {
            this.errors.push(e);
          });
      }

      this.close();
    },

    saveProjects() {
      HTTP.post(`projects`, this.editedItem)
        .then(res => {
          this.loadProjects();
          this.loadForm();
        })
        .catch(e => {
          this.errors.push(e);
        });
    },

    loadProjects() {
      HTTP.get(`projects`)
        .then(res => {
          console.log("res", res);
          this.projects = res.data;
        })
        .catch(e => {
          this.errors.push(e);
        });
    },

    loadForm() {
      this.editedItem = {
        investor: "",
        project: "",
        investment: "",
        time: ""
      };
    }
  }
};
</script>
