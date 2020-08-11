<template>
  <div>
    <section v-if="errored">
      <p>
        Can't get data from API endpoint
      </p>
    </section>

    <section v-else>
      <div v-if="loading">Loading...</div>

      <div v-else>
        <v-toolbar flat>
          <div v-for="item in items" :key="item.id" class="item">
            <v-toolbar-title>{{ item.name }}</v-toolbar-title>
          </div>

          <v-divider class="mx-4" inset vertical></v-divider>
          <v-spacer></v-spacer>
          <v-dialog v-model="dialog" max-width="500px">
            <template v-slot:activator="{ on, attrs }">
              <v-btn dark v-bind="attrs" v-on="on">New Item</v-btn>
              <v-btn dark @click="toggleGrid" :class="{ active: activeGrid }">
                <v-icon small>
                  mdi-apps
                </v-icon></v-btn
              >

              <v-btn dark @click="togglePanel" :class="{ active: activePanel }">
                <v-icon small>
                  mdi-view-sequential
                </v-icon></v-btn
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

        <br /><br />

        <v-item-group class="grid-layout" :class="{ active: activeGrid }">
          <v-container>
            <v-row>
              <v-col
                v-for="product in products"
                :key="product.id"
                cols="12"
                md="4"
              >
                <v-item>
                  <v-card class="mx-auto" max-width="344" outlined>
                    <v-list-item three-line>
                      <v-list-item-content>
                        <div class="overline mb-4">
                          {{ product.data.category }}
                        </div>
                        <v-list-item-title class="headline mb-1">{{
                          product.data.title
                        }}</v-list-item-title>
                        <br /><br />
                        <v-list-item-subtitle
                          >{{ product.data.description }}... There are many
                          variations of passages of Lorem Ipsum available, but
                          the majority have suffered alteration in some form, by
                          injected humour, or randomised words which don't look
                          even slightly believable.</v-list-item-subtitle
                        >
                        <br /><br />
                        <div class="overline mb-4">
                          Price: {{ product.data.price }} €
                        </div>
                      </v-list-item-content>

                      <v-list-item-avatar
                        tile
                        size="80"
                        color="grey"
                      ></v-list-item-avatar>
                    </v-list-item>

                    <v-card-actions>
                      <router-link
                        :to="{
                          name: 'details',
                          params: {
                            id: product.id,
                            title: product.data.title,
                            description: product.data.description,
                            price: product.data.price,
                            category: product.data.category,
                          },
                        }"
                      >
                        <v-btn text>
                          Read more
                        </v-btn>
                      </router-link>
                      <a
                        href="javascript:void"
                        @click.prevent="deleteItem(product)"
                      >
                        <v-btn text>
                          Delete
                        </v-btn>
                      </a>
                    </v-card-actions>
                  </v-card>
                </v-item>
              </v-col>
            </v-row>
          </v-container>
        </v-item-group>

        <v-expansion-panels
          popout
          class="panel-layout"
          :class="{ active: activePanel }"
        >
          <v-expansion-panel
            v-for="product in products"
            :key="product.id"
            hide-actions
          >
            <v-expansion-panel-header>
              <v-row align="center" class="spacer" no-gutters>
                <v-col cols="4" sm="2" md="1">
                  <v-avatar size="36px">
                    <img
                      alt="Avatar"
                      src="https://images2.minutemediacdn.com/image/upload/c_crop,h_842,w_1500,x_0,y_88/f_auto,q_auto,w_1100/v1555006794/shape/mentalfloss/istock-522735736.jpg"
                    />
                  </v-avatar>
                </v-col>

                <v-col class="hidden-xs-only" sm="5" md="3">
                  <strong v-html="product.data.title"></strong>
                  <span class="grey--text">
                    &nbsp;({{ product.data.category }})
                  </span>
                </v-col>

                <v-col class="text-no-wrap" cols="5" sm="3">
                  <strong v-html="product.data.employee"></strong>
                </v-col>

                <v-col
                  v-if="product.data.category"
                  class="grey--text text-truncate hidden-sm-and-down"
                >
                  Price: {{ product.data.price }} €
                </v-col>
              </v-row>
            </v-expansion-panel-header>

            <v-expansion-panel-content>
              <v-divider></v-divider>
              <v-card-text v-text="product.data.description">
                <span
                  >... There are many variations of passages of Lorem Ipsum
                  available, but the majority have suffered alteration in some
                  form, by injected humour, or randomised words which don't look
                  even slightly believable.</span
                >
              </v-card-text>
            </v-expansion-panel-content>
          </v-expansion-panel>
        </v-expansion-panels>
      </div>
    </section>
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
    defaultProduct: {
      title: '',
      description: '',
      price: '',
      category: '',
      employee: '',
    },

    loading: true,
    errored: false,
    activeGrid: false,
    activePanel: false,
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
      axios
        .get(api)
        .then((response) => {
          // console.log(response)
          this.items = response
        })
        .catch((error) => {
          console.log(error)
          this.errored = true
        })
        .finally(() => (this.loading = false))
    },

    fetchProducts() {
      const productsApi =
        'http://us-central1-test-b7665.cloudfunctions.net/api/stores/ijpxNJLM732vm8AeajMR/products/'
      axios
        .get(productsApi)
        .then((response) => {
          // console.log(response)
          this.products = response.data
        })
        .catch((error) => {
          console.log(error)
          this.errored = true
        })
        .finally(() => (this.loading = false))
    },

    deleteItem(item) {
      // console.log(item)
      const productsApi =
        'http://us-central1-test-b7665.cloudfunctions.net/api/stores/ijpxNJLM732vm8AeajMR/products/'
      const index = this.products.indexOf(item)
      confirm('Are you sure you want to delete this item?') &&
        this.products.splice(index, 1)
      // console.log('deleted data')

      axios
        .delete(productsApi + item.id)
        .then((response) => {
          // console.log(response)
        })
        .catch((error) => {
          console.log(error)
          this.errored = true
        })
        .finally(() => (this.loading = false))
    },

    close() {
      this.dialog = false
      this.$nextTick(() => {
        this.productItems = Object.assign({}, this.defaultProduct)
      })
    },

    save(item) {
      const productsApi =
        'http://us-central1-test-b7665.cloudfunctions.net/api/stores/ijpxNJLM732vm8AeajMR/products/'

      // console.log('created data')
      // console.log(this.productItems)

      axios
        .post(productsApi, {
          title: this.productItems.title,
          description: this.productItems.description,
          price: this.productItems.price,
          category: this.productItems.category,
          employee: this.productItems.employee,
        })
        .then((response) => {
          // console.log(response)
        })
        .catch((error) => {
          console.log(error)
          this.errored = true
        })
        .finally(() => (this.loading = false))

      this.products.push(this.productItems)
      this.close()
    },

    toggleGrid() {
      this.activeGrid = !this.activeGrid
      this.activePanel = false
    },
    togglePanel() {
      this.activePanel = !this.activePanel
      this.activeGrid = true
    },
  },
}
</script>

<style scoped>
.v-toolbar__content button {
  margin-right: 8px !important;
}

a {
  text-decoration: none;
}

.grid-layout {
  transition: all 0.25s;
  position: absolute;
  top: 80px;
}

.grid-layout.active {
  opacity: 0;
  visibility: hidden;
  position: absolute;
  top: 80px;
}

.panel-layout {
  transition: all 0.25s;
  opacity: 0;
  visibility: hidden;
}

.panel-layout.active {
  opacity: 1;
  visibility: visible;
}
</style>
