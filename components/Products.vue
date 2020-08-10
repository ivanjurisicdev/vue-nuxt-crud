<template>
  <div>
    <v-data-table
      :headers="headers"
      :items="products"
      disable-sort
      hide-default-footer
      disable-pagination
      class="elevation-1"
    >
      <template v-slot:top>
        <v-toolbar flat>
          <div v-for="item in items" :key="item.id" class="item">
            <v-toolbar-title>{{ item.name }}</v-toolbar-title>
          </div>

          <v-divider class="mx-4" inset vertical></v-divider>
          <v-spacer></v-spacer>
          <v-dialog v-model="dialog" max-width="500px">
            <template v-slot:activator="{ on, attrs }">
              <v-btn color="primary" dark class="mb-2" v-bind="attrs" v-on="on"
                >New Item</v-btn
              >
            </template>
            <v-card>
              <v-card-title>
                <span class="headline">New Item</span>
              </v-card-title>

              <v-card-text>
                <v-container>
                  <v-row>
                    <v-col cols="12" sm="6" md="4">
                      <v-text-field
                        v-model="productItems.title"
                        label="Title"
                      ></v-text-field>
                    </v-col>
                    <v-col cols="12" sm="6" md="4">
                      <v-text-field
                        v-model="productItems.description"
                        label="Description"
                      ></v-text-field>
                    </v-col>
                    <v-col cols="12" sm="6" md="4">
                      <v-text-field
                        v-model="productItems.price"
                        label="Price"
                      ></v-text-field>
                    </v-col>
                    <v-col cols="12" sm="6" md="4">
                      <v-text-field
                        v-model="productItems.category"
                        label="Category"
                      ></v-text-field>
                    </v-col>
                    <v-col cols="12" sm="6" md="4">
                      <v-text-field
                        v-model="productItems.employee"
                        label="Employee"
                      ></v-text-field>
                    </v-col>
                  </v-row>
                </v-container>
              </v-card-text>

              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn color="blue darken-1" text @click="close">Cancel</v-btn>
                <v-btn color="blue darken-1" text @click="save">Save</v-btn>
              </v-card-actions>
            </v-card>
          </v-dialog>
        </v-toolbar>
      </template>
      <template v-slot:item.actions="{ item }">
        <v-icon small @click="deleteItem(item)">
          mdi-delete
        </v-icon>
        <router-link
          :to="{
            name: 'details',
            params: {
              id: item.id,
              title: item.data.title,
              description: item.data.description,
              price: item.data.price,
              category: item.data.category,
            },
          }"
        >
          <v-icon small class="mr-2">
            mdi-eye-outline
          </v-icon>
        </router-link>
      </template>
    </v-data-table>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  data: () => ({
    dialog: false,
    headers: [
      { text: 'Id', value: 'id' },
      { text: 'Title', value: 'data.title' },
      { text: 'Description', value: 'data.description' },
      { text: 'Price', value: 'data.price' },
      { text: 'Category', value: 'data.category' },
      { text: 'Employee', value: 'data.employee' },
      { text: 'Actions', value: 'actions' },
    ],
    items: [],
    products: [],
    productItems: {
      title: '',
      description: '',
      price: '',
      category: '',
      employee: '',
    },
  }),

  watch: {
    dialog(val) {
      val || this.close()
    },
  },

  created() {
    this.fetchStore()
    this.fetchProducts()
  },

  methods: {
    fetchStore() {
      const api =
        'http://us-central1-test-b7665.cloudfunctions.net/api/stores/ijpxNJLM732vm8AeajMR'
      axios.get(api).then((response) => {
        console.log(response)
        this.items = response
      })
    },

    fetchProducts() {
      const productsApi =
        'http://us-central1-test-b7665.cloudfunctions.net/api/stores/ijpxNJLM732vm8AeajMR/products/'
      axios.get(productsApi).then((response) => {
        console.log(response)
        this.products = response.data
      })
    },

    deleteItem(item) {
      console.log(item)
      const productsApi =
        'http://us-central1-test-b7665.cloudfunctions.net/api/stores/ijpxNJLM732vm8AeajMR/products/'
      const index = this.products.indexOf(item)
      confirm('Are you sure you want to delete this item?') &&
        this.products.splice(index, 1)
      console.log('deleted data')

      axios.delete(productsApi + item.id).then((response) => {
        console.log(response)
      })
    },

    close() {
      this.dialog = false
    },

    save(item) {
      const productsApi =
        'http://us-central1-test-b7665.cloudfunctions.net/api/stores/ijpxNJLM732vm8AeajMR/products/'

      console.log('created data')
      console.log(this.productItems)

      axios
        .post(productsApi, {
          title: this.productItems.title,
          description: this.productItems.description,
          price: this.productItems.price,
          category: this.productItems.category,
          employee: this.productItems.employee,
        })
        .then((response) => {
          console.log(response)
        })

      this.products.push(this.productItems)
      this.close()
    },
  },
}
</script>
