<template>
    <div>
      <Nav></Nav>
      <v-container v-if="product">
        <v-row justify="center">
          <v-col cols="11">
            <h2 class="text-center text-md-h4 font-weight-bold">
              {{ product.name }}
            </h2>
            <div class="mt-2 text-center">
              <v-rating
                readonly
                half-increments
                class="mb-2"
                color="yellow darken-2"
                background-color="grey lighten-1"
                :value="product.ratings"
                dense
                size="20"
              ></v-rating>
              <v-chip
                small
                label
                outlined
                class="mr-1"
                v-for="(t, i) in product.tags"
                :key="`prod${product.id}-${i}`"
              >
                {{ t }}
              </v-chip>
            </div>
            <br />
            <v-row>
              <v-col cols="7">
                <div class="custommug">
                  <v-img
                    :src="product.image"
                  >
                    <svg
                      v-if="product.id === 1"
                      width="100%"
                      height="100%"
                      viewBox="0 0 500 300"
                      preserveAspectRatio="xMinYMin meet"
                    >
                      <foreignObject x="170" y="120" width="100" height="100" class="custommug-text">
                        {{customtext}}
                      </foreignObject>
                    </svg>
                  </v-img>  
                </div>
              </v-col>
              <v-col cols="5">
                <h4 v-if="product.id === 1" >Custom Text:</h4>
                <br />
                <v-text-field
                v-if="product.id === 1"
                outlined
                clearable
                filled
                v-model="customtext"
                >
                </v-text-field>
                <v-btn
                  @click="$store.commit('cart/AddToCart', product)"
                  min-height="45"
                  min-width="170"
                  class="text-capitalize justify-center"
                  color="primary"
                  >Add To Cart</v-btn
                >
                <p class="mt-5 mb-7">
                  {{ product.description }}
                </p>
              </v-col>
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
      let d = await this.$content("products")
        .where({ id: parseInt(this.$route.params.id) })
        .limit(1)
        .fetch();
      this.product = d[0];
    },
    data() {
      return {
        product: null,
        customtext: 'I Love Coffee',
      };
    },
  };
  </script>
  
  <style></style>