<template>
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
                  <v-text-field
                    v-model="editedItem.investor"
                    label="Chủ đầu tư"
                  ></v-text-field>
                </v-flex>
                <v-flex xs12 sm6 md12>
                  <v-text-field
                    v-model="editedItem.project"
                    label="Dự án"
                  ></v-text-field>
                </v-flex>
                <v-flex xs12 sm6 md12>
                  <v-text-field
                    v-model="editedItem.investment"
                    label="Số vốn (tỷ)"
                  ></v-text-field>
              
                </v-flex>
                <v-flex xs12 sm6 md12>
                  <v-text-field
                    v-model="editedItem.time"
                    label="Thời gian triển khai (tháng)"
                  ></v-text-field>
                </v-flex>
              </v-layout>
            </v-container>
          </v-card-text>
          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn color="blue darken-1" flat @click.native="close"
              >Cancel</v-btn
            >
            <v-btn color="blue darken-1" flat @click.native="save(editedItem.id)">Save</v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
</template>

<script>
export default {
    name: 'dialog',
    name: 'dialog',
}
</script>

<style>

</style>