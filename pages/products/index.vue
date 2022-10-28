<template>
    <div>
      <Nav></Nav>
      <v-container>
        <v-row dense>
          <v-col md="3">
            <div style="position: sticky; top: 92px">
              <v-text-field
                prepend-inner-icon="mdi-magnify"
                outlined
                v-model="search"
                clearable
                placeholder="Search"
              ></v-text-field>
              <v-list
                v-if="$vuetify.breakpoint.mdAndUp"
                color="transparent"
                subheader
              >
                <v-subheader>Categories</v-subheader>
                <v-list-item
                  link
                  @click="catagoryclick(c)"
                  v-for="(c, i) in categories"
                  :key="`category${i}`"
                >
                  <v-list-item-avatar>
                    <v-img :src="c.image"></v-img>
                  </v-list-item-avatar>
                  <v-list-item-content>
                    <v-list-item-title>
                      {{ c.name }}
                    </v-list-item-title>
                  </v-list-item-content>
                </v-list-item>
              </v-list>
            </div>
          </v-col>
          <v-col md="9">
            <v-row>
              <template >
                <v-fade-transition v-for="(p, i) in filteredProducts" :key="`product${p.id}-${i}`">
                  <v-col cols="12" md="6">
                    <v-card
                      nuxt
                      :to="`/products/${p.id}`"
                      color="surface"
                      class="el ma-2 mb-5 mr-5"
                    >
                      <v-img :src="p.image" height="300">
                        <template #placeholder>
                          <v-row
                            class="fill-height"
                            justify="center"
                            align="center"
                          >
                            <v-progress-circular
                              width="2"
                              size="100"
                              color="primary"
                              indeterminate
                            ></v-progress-circular>
                          </v-row>
                        </template>
                      </v-img>
                      <v-card-title class="text-md-body-1 font-weight-bold">{{
                        p.name
                      }}</v-card-title>
                      <v-card-subtitle class="primary--text pb-3">
                        ${{ p.price }}
                      </v-card-subtitle>
                      <v-card-text>
                        <v-chip
                          x-small
                          label
                          outlined
                          class="mr-1"
                          v-for="(t, i) in p.tags"
                          :key="`prod${p.id}-${i}`"
                        >
                          {{ t }}
                        </v-chip>
                      </v-card-text>
                    </v-card>
                  </v-col>
                </v-fade-transition>
              </template>
            </v-row>
          </v-col>
        </v-row>
      </v-container>
      <br /><br />
      <Footer />
      <ScrollTop />
    </div>
  </template>
  
  <script>
  export default {
    async created() {
      this.products = await this.$content("products").fetch();
      this.categories = await this.$content("category").fetch();
    },
    data() {
      return {
        products: null,
        categories: null,
        search: null,
      };
    },
    computed: {
      console: () => console,
      filteredProducts() {
        if (!this.products || !this.search) return this.products;
        return this.products.filter((p) => {
          const s = this.search.toLowerCase();
          const n = p.name.toLowerCase();
          const price = p.price.toString();
          const r = p.ratings.toString();
          const tags = p.tags.toString().toLowerCase();
          return (
            n.includes(s) ||
            price.includes(s) ||
            r.includes(s) ||
            tags.includes(s)
          );
        });
      },
    },
    methods: {
      catagoryclick(c) {
        this.search = c.name.toLowerCase();
      },
    },
  };
  </script>
  
  <style></style>