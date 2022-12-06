<template>
  <div class="background">
    <div v-show="loading" class="loader"></div>
    <div id="bg-pattern" v-bind:class="ClassCategory(productsCategory)"></div>

    <div
      class="products-invalid"
      v-if="ClassCategory(productsCategory) == 'product-invalid'"
    >
      <div class="products-invalid-content">
        <h3>This product is unavailable to show</h3>
        <button @click="nextPage">Next Product</button>
      </div>
    </div>

    <div
      class="products-content"
      v-bind:style="{
        display:
          ClassCategory(productsCategory) == 'product-invalid' ? 'none' : '',
      }"
    >
      <div class="photo">
        <img :src="products.image" alt="product image" />
      </div>
      <div class="detail">
        <div class="row1">
          <p class="title" :style="{ color: StyleCategory(productsCategory) }">
            {{ products.title }}
          </p>
          <div class="category">
            <p>{{ productsCategory }}</p>
            <div class="rating">
              <p>{{ rating }}/5</p>
              <div class="rating-item">
                <div class="rating-scale" v-for="rating in 5" :key="rating">
                  <div
                    v-if="rating <= ratingCeil"
                    class="fill"
                    :style="{
                      backgroundColor: StyleCategory(productsCategory),
                      color: StyleCategory(productsCategory),
                      borderStyle: 'Solid',
                      borderWidth: '3px',
                      borderColor: StyleCategory(productsCategory),
                    }"
                  ></div>
                  <div
                    v-else
                    class="no-fill"
                    :style="{
                      color: StyleCategory(productsCategory),
                      borderWidth: '1px',
                      borderStyle: 'Solid',
                      borderColor: StyleCategory(productsCategory),
                    }"
                  ></div>
                </div>
              </div>
            </div>
          </div>
          <hr />
        </div>
        <div class="row2">
          <p>{{ products.description }}</p>
        </div>
        <div class="row3">
          <hr />
          <p :style="{ color: StyleCategory(productsCategory) }">
            ${{ products.price }}
          </p>
          <button
            id="bl"
            :style="{
              backgroundColor: StyleCategory(productsCategory),
              color: 'var(--putih)',
            }"
          >
            Buy now
          </button>
          <button
            @click="nextPage"
            :style="{
              color: StyleCategory(productsCategory),
              borderWidth: '3px',
              borderColor: StyleCategory(productsCategory),
            }"
          >
            Next Product
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "ProductDisplay",
  data() {
    return {
      loading: false,
      products: [],
      productsCategory: String,
      rating: [],
      index: 1,
      ratingCeil: 1,
    };
  },

  mounted() {
    this.fetchProduct();
  },

  methods: {
    fetchProduct() {
      this.loading = true;
      fetch(`https://fakestoreapi.com/products/${this.index}`)
        .then((res) => res.json())
        .then((data) => {
          this.loading = false;
          this.products = data;
          this.productsCategory = data.category;
          this.rating = data.rating.rate;
          this.ratingCeil = Math.ceil(data.rating.rate);
        })
        .catch((err) => console.log(err.message));
    },
    nextPage() {
      this.index++;
      if (this.index > 20) {
        this.index = 1;
        this.fetchProduct();
      }
      this.fetchProduct();
    },
    StyleCategory(productsCategory) {
      if (productsCategory == "men's clothing") return "var(--birutua)";
      else if (productsCategory == "women's clothing") return "var(--ungutua)";
      else return "var(--abu)";
    },
    ClassCategory(productsCategory) {
      if (productsCategory == "men's clothing") return "men-clothing";
      else if (productsCategory == "women's clothing") return "women-clothing";
      else return "product-invalid";
    },
  },
};
</script>
