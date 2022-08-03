<template>
     <v-data-table
      :headers="headers"
      :items="projects"
      :search="search"
      class="elevation-1"
    >
      <template slot="items" slot-scope="props">
        <td class="text-xs-left">{{ props.item.investor }}</td>
        <td class="text-xs-left">{{ props.item.project }}</td>
        <td class="text-xs-left">{{ props.item.investment }}</td>
        <td class="text-xs-left">{{ props.item.time }}</td>
        <td class="layout">
          <v-icon small class="mr-2" @click="editItem(props.item)">
            edit</v-icon
          >
          <v-icon small @click="deleteItem(props.item.id)"> delete</v-icon>
        </td>
      </template>
      <v-alert slot="no-results" :value="true" color="error" icon="warning">
        Your search for "{{ search }}" found no results.
      </v-alert>
    </v-data-table>
</template>

<script>
export default {
    name: 'TableTodo',
    props: {
        headers: {
            type: Array,
            required: true
        },
        projects: {
            type: Array,
            required: true
        },
        search: {
            type: String,
            required: true
        }
    },
    data() {
      return {

      }
    },
    methods: {
        editItem(item) {
            this.$emit('editItem', {item :item} )
        },
        deleteItem() {
          this.$emit('deleteItem')
        }
    }
}
</script>