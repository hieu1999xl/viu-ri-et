<template>
<div>
   <v-toolbar flat>
      <v-card-title class='title'>
        CRUD PROJECT
      </v-card-title>
        <v-spacer></v-spacer>
           <v-flex xs12 sm6 md3>
        <v-text-field align-center v-model="search" clearable append-icon="search" label="Search" single-line hide-details></v-text-field>  
        </v-flex>   
        <v-dialog v-model="dialog" max-width="500px">
          <v-btn slot="activator" color="dark" dark class="mb-2">New Item</v-btn>
          <v-card>
            <v-card-title>
              <span class="headline">{{ formTitle }}</span>
            </v-card-title>
  
            <v-card-text>
              <v-container grid-list-md>
                <v-layout wrap>
                  <v-flex xs12 sm6 md12>
                    <v-text-field v-model="editedItem.investor" label="Chủ đầu tư"></v-text-field>
                  </v-flex>
                  <v-flex xs12 sm6 md12>
                    <v-text-field v-model="editedItem.project" label="Dự án"></v-text-field>
                  </v-flex>
                  <v-flex xs12 sm6 md12>
                    <v-text-field v-model="editedItem.investment" label="Số vốn (tỷ)"></v-text-field>
                  </v-flex>
                  <v-flex xs12 sm6 md12>
                    <v-text-field v-model="editedItem.time" label="Thời gian triển khai (tháng)"></v-text-field>
                  </v-flex>
                </v-layout>
              </v-container>
            </v-card-text>
  
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="blue darken-1" flat @click.native="close">Cancel</v-btn>
              <v-btn color="blue darken-1" flat @click.native="save">Save</v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>
      </v-toolbar>
      
      <v-data-table  :headers="headers" :items="projects" :search="search" class="elevation-1">
        <template slot="items" slot-scope="props">
          <td class="text-xs-left">{{ props.item.investor }}</td>
          <td class="text-xs-left">{{ props.item.project }}</td>
          <td class="text-xs-left">{{ props.item.investment }}</td>
          <td class="text-xs-left">{{ props.item.time }}</td>
          <td class="layout">
            <v-icon small class="mr-2" @click="editItem(props.item)"> edit</v-icon>
            <v-icon small @click="deleteItem(props.item)"> delete</v-icon>
          </td>
        </template>
        <template slot="no-data"><v-btn color="primary" @click="initialize">Reset</v-btn></template>
        <v-alert slot="no-results" :value="true" color="error" icon="warning">
          Your search for "{{ search }}" found no results.
        </v-alert>
      </v-data-table>
</div>
</template>


<script>
export default {
    data: () => ({
    dialog: false,
    search: '',
    headers: [
      { text: 'Chủ đầu tư', align: 'left', sortable: true, value: 'investor' },
      { text: 'Dựa án', value: 'project' },
      { text: 'Số vốn (tỷ)', value: 'investment' },
      { text: 'Thời hạn (tháng)', value: 'time' },
      { text: 'Hành động', value: 'name', sortable: false }
    ],
    projects: [],
    editedIndex: -1,
    editedItem: {
      investor: '',
      project: '',
      investment: '',
      time: ''
    },
    defaultItem: {
      investor: '',
      project: '',
      investment: '',
      time: ''
    }
  }),
  computed: {    
    formTitle () {
      return this.editedIndex === -1 ? 'New Item' : 'Edit Item'
    }
  },
  
  created () {
    this.loadAlbums()
  },
  mounted() {
    
  },
  watch: {
    dialog (val) {
      val || this.close()
    }
  },
  computed: {    
    formTitle () {
      return this.editedIndex === -1 ? 'New Item' : 'Edit Item'
    }
  },
  methods: {
    editItem (item) {
      this.editedIndex = this.projects.indexOf(item)
      this.editedItem = Object.assign({}, item)
      this.saveProjects();
      this.dialog = true
    },
    deleteItem (item) {
      const index = this.projects.indexOf(item)
      if ( confirm("Are you sure you want to delete this item?") === true) {
        this.projects.splice(index, 1)
        alert('Deleted Album')
        this.saveProjects()
      } else {
        alert('ABORTED Delete Album')
      }      
      // confirm('Are you sure you want to delete this item?') && this.projects.splice(index, 1)
      // this.saveProjects();
    },
    close () {
      this.dialog = false
      setTimeout(() => {
        this.editedItem = Object.assign({}, this.defaultItem)
        this.editedIndex = -1
      }, 300)
    },
    save () {
      if (this.editedIndex > -1) {
        Object.assign(this.projects[this.editedIndex], this.editedItem)
        //this.saveProjects();
      } else {
        this.projects.push(this.editedItem)
        //this.saveProjects();
      }
      this.saveProjects();
      this.close()    
    },    
    saveProjects() {
      alert('Saving ALL projects to Local Storage')
      let parsed = JSON.stringify(this.projects);
      localStorage.setItem('projects', parsed);
    },
    loadAlbums() {
      // Loads localStorage.getItem('projects'))
      if(localStorage.getItem('projects')) {
        try {
          this.projects = JSON.parse(localStorage.getItem('projects'));
        } catch(e) {
          localStorage.removeItem('projects');
        }
      }      
    },
  }
}
</script>