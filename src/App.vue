<template>
  <div id="app">
    <header class="header" :class="{ 'is-hidden': !showHeader }">
      <div class="container">
        <a href="#" class="logo"><img src="./assets/logo.svg"></a>
        <h3>Kaffe & Espresso</h3>
        <ProductFilter :products='items' @emitFilter="filterBtnHandler" />
      </div>
    </header>
    <main id="main">
      <section class="catalog">
        <div class="container">
          <h5>Artikel 10</h5>
          <div class="catalog-list">
            <ProductItem v-for="item of filteredItems" :key="item._id" :product="item" />
          </div>
        </div>
      </section>
    </main>
  </div>
</template>

<script>
import ProductItem from './components/ProductItem';
import ProductFilter from './components/ProductFilter';

export default {
  components: {
    ProductItem,
    ProductFilter,
  },
  data() {
    return {
      items: [],
      filteredItems: [],
      showHeader: true,
      lastScrollPosition: 0,
      scrollOffset: 40
    };
  },
  methods: {
    async getData() {
      try {
        const url = "https://www.roastmarket.de/api/catalog/vue_storefront_catalog/product/_search?";
        let response = await fetch(url);
        let json = await response.json();
        this.items = json.hits.hits;
      } catch (error) {
        console.error("HTTP Error: " + response.status);
      }
    },
    filterBtnHandler(e) {
      switch (e) {
        case 1:
          this.getDefaultItems()
          break
        case 2:
          this.sortByPrice()
          break
        case 3:
          this.sortByRating()
          break
        default:
          this.getDefaultItems()
      }
    },
    sortByRating() {
      this.filteredItems.sort((productA, productB) => productB._source.yotpo_rating - productA._source.yotpo_rating);
      this.sortInStock();
    },
    sortByPrice() {
      this.filteredItems.sort((productA, productB) => productB._source.base_price_amount - productA._source.base_price_amount);
      this.sortInStock();
    },
    sortInStock() {
      this.filteredItems.sort((productA, productB) => productB._source.stock.is_in_stock - productA._source.stock.is_in_stock)
    },
    getDefaultItems() {
      this.filteredItems = [...this.items]
      this.sortInStock()
    },
    hideOnScroll() {
      if (window.pageYOffset < 0) {
        return
      }
      if (Math.abs(window.pageYOffset - this.lastScrollPosition) < this.scrollOffset) {
        return
      }
      this.showHeader = window.pageYOffset < this.lastScrollPosition
      this.lastScrollPosition = window.pageYOffset
    }
  },
  async mounted() {
    await this.getData();
    this.filterBtnHandler()
    this.lastScrollPosition = window.pageYOffset
    window.addEventListener('scroll', this.hideOnScroll)
  },
  beforeDestroy() {
    window.removeEventListener('scroll', this.hideOnScroll)
  },
}
</script>

<style lang="scss">
body {
  margin: 0;
}

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #000;
}

h2,
h3,
h5 {
  font-weight: 700;
  margin: 0 0 16px;
}


a {
  color: #000;
  text-decoration: none;
  transition: .2s;

  &:hover {
    opacity: .8;
  }
}

img {
  max-width: 100%;
  height: auto;
}

#main {
  margin-top: 145px;

  @media only screen and (min-width : 768px) {
    margin-top: 0;
  }
}

.container {
  max-width: 1240px;
  padding-right: 20px;
  padding-left: 20px;
  margin: 0 auto;
}

.header {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  background: #f8f7f5;
  padding: 15px 0;
  z-index: 1;
  transform: translateY(0);
  transition: transform 300ms linear;

  @media only screen and (min-width : 768px) {
    position: static;
    padding: 30px 0;
    transform: none;
  }

  &.is-hidden {
    transform: translateY(-100%);

    @media only screen and (min-width : 768px) {
      transform: translateY(0);
    }
  }

  .container {
    text-align: left;

    @media only screen and (min-width : 768px) {
      display: flex;
      flex-wrap: wrap;
      align-items: center;
    }
  }

  h3 {
    margin-bottom: 20px;

    @media only screen and (min-width : 768px) {
      margin-bottom: 0;
      display: block;
      width: 100%;
      text-align: left;
      order: 1;
    }
  }
}

.logo {
  max-width: 135px;
  width: 100%;
  margin-right: 20px;
  margin-bottom: 20px;
  display: inline-block;
  vertical-align: top;
}
</style>
