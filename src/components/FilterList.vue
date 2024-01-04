<template>
  <section class="products">
    <div class="filters-container">
      <form class="input-form">
        <input type="text" class="search-input" @input="handleSearch($event)" />
      </form>
      <h5>Company</h5>
      <article class="companies">
        <button
          v-for="company in buttons"
          :key="company"
          class="company-btn"
          :data-id="company"
          @click="handleButton"
        >
          {{ company }}
        </button>
      </article>
    </div>
    <div class="products-container">
      <template v-if="filteredProduct.length">
        <article
          class="product"
          v-for="product in filteredProduct"
          :key="product"
          :data-id="product.id"
        >
          <img :src="product.image" alt="" className="product-img img" />
          <footer>
            <h5 className="product-name">{{ product.title }}</h5>
            <span className="product-price">{{ product.price }}</span>
          </footer>
        </article>
      </template>
      <h6 v-else>Sorry, no products matched your search</h6>
    </div>
  </section>
</template>

<script>
import "../assets/style.css";
import { products } from "../mocks/Product";

export default {
  name: "FilterList",
  data() {
    return {
      filteredProduct: products,
      products,
      buttons: [],
      inputValue: "",
    };
  },
  created() {
    this.buttons = [
      "all",
      ...new Set(
        this.products.map((product) => {
          return product.company;
        })
      ),
    ];
  },
  methods: {
    handleButton(event) {
      const target = event.target;
      const datasetId = target.dataset.id;

      if (datasetId === "all") {
        this.filteredProduct = products;
      } else {
        const filteredList = this.products.filter((product) => {
          return product.company === datasetId;
        });

        this.filteredProduct = filteredList;
      }
    },
    handleSearch(event) {
      this.inputValue = event.target.value;

      const filterList = products.filter((product) => {
        return product.title.toLowerCase().includes(this.inputValue);
      });

      this.filteredProduct = filterList;
    },
  },
};
</script>
