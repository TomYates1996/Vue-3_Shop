<template>
  <div class="shop-home-con flex-col">
    <Header :payment="this.showPayment" :basket="this.basket" @filter-products="filterProducts" 
    @go-home="home" @increase-amount="changeQuantity" @payment="paymentPage" 
    @big-basket="checkout" @delete-item="deleteItem" />
    <div class="main-page-wrap flex" v-if="homePage">
      <ProductContainer :products="this.products" @add-to-basket="addToBasket"/>
    </div>
    <Checkout v-if="checkoutPage" :basket="this.basket" @clear="emptyBasket" @delete-item="deleteItem"
     @close="closeCheckout" @increase-amount="changeQuantity" @card-details="paymentPage"/>
    <PaymentPage v-if="showPayment" :basket="this.basket" @delete-item="deleteItem" @place-order="placeOrder" />
    <ReceiptPage v-if="receipt" :details="this.buyDetails" :basket="this.basket" />
  </div>
</template>

<script>
import Header from './components/Header'
import ProductContainer from './components/ProductContainer'
import Checkout from './components/Checkout'
import PaymentPage from './components/PaymentPage'
import ReceiptPage from './components/ReceiptPage'

export default {
  name: 'App',
  components: {
    Header,
    ProductContainer,
    Checkout,
    PaymentPage,
    ReceiptPage,
  }, 
  data() {
    return {
      test: '',
      products: [
        {name: 'Orange', image: 'orange.jpg', rating: 90, raters: 2425, description: 'An orange fruit, tastes like orange', amount: 10, price: 0.4},
        {name: 'Kiwi', image: 'kiwi.jpg', rating: 100, raters: 21129, description: 'A green fruit, tastes like kiwi', amount: 30, price: 0.2},
        {name: 'Banana', image: 'banana.jpg', rating: 64, raters: 1203, description: 'A yellow fruit, tastes like milk', amount: 27, price: 0.17},
        {name: 'Watermelon', image: 'watermelon.png', rating: 60, raters: 982, description: 'A green shelled fruit, tastes like watermelon', amount: 110, price: 1.00},
        {name: 'Orange-Small', image: 'orange.jpg', rating: 85, raters: 64, description: 'An orange fruit, tastes like orange', amount: 10, price: 0.2},
        {name: 'Kiwi-Jumbo', image: 'kiwi.jpg', rating: 100, raters: 29, description: 'A green fruit, tastes like kiwi', amount: 30, price: 0.43},
        {name: 'Microbanana', image: 'banana.jpg', rating: 34, raters: 13, description: 'A yellow fruit, tastes like milk', amount: 27, price: 0.1},
        {name: 'Wetmelon', image: 'watermelon.png', rating: 9, raters: 11, description: 'A green shelled fruit, tastes like watermelon', amount: 110, price: 19.00},
      ],
      basket: [],
      basketShow: false,
      checkoutPage: false,
      showPayment: false,
      receipt: false,
      buyDetails: [],
      homePage: true,
      count: 0,
    }
  },
  methods: {
    filterProducts(search) {
      this.test = search
    },
    addToBasket(q, p) {
      let inAlready = false;
      this.basket.forEach (el => {
        if (el.product == p) {
          el.quantity = (el.quantity + q);
          inAlready = true;
        } 
      })
      if (inAlready == false) {
        this.basket = [...this.basket, {product: p, quantity: q}]
      }
      this.count = this.count + q
    },
    emptyBasket() {
      this.basket = []
    },
    deleteItem(item) {
      this.basket.splice(this.basket.indexOf(item), 1);
    },
    // closeBasket(newBasket) {
    //   this.homePage = true;
    //   this.basketShow = false;
    //   this.basket = newBasket;
    // },
    checkout() {
      // this.basketShow = false;
      this.checkoutPage = true;
      this.homePage = false;
    },
    closeCheckout() {
      this.checkoutPage = false;
      this.homePage = true;
    },
    paymentPage() {
      this.showPayment = true;
      this.checkoutPage = false;
      this.homePage = false;
    },
    placeOrder(details) {
      this.showPayment = false;
      this.buyDetails = details;
      this.receipt = true;
      this.basket = []
    },
    home() {
      this.homePage = true;
      this.receipt = false;
      this.buyDetails = false;
      this.showPayment = false;
      this.checkoutPage = false;
    },

    // (total - oldQuantity + newQuantity)
    changeQuantity(q, p) {
      this.basket.forEach (el => {
        if (el.product == p) {
          this.count = this.count - el.quantity
          el.quantity = (q);
          this.count = this.count + q
        } 
      })
    }
  }
}
</script>

<style>
  @import url('https://fonts.googleapis.com/css2?family=Poppins:wght@100;200;300;400;500;600;700;800;900&display=swap');
  *{
    box-sizing: border-box;
    padding: 0;
    margin: 0;
    font-family: 'Poppins', sans-serif;
  }
  .grid {
    display: grid;
  }
  .flex {
    display: flex;
    align-items: center;
    justify-content: center;
  }
  .flex-col {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: flex-start;
  }
  .shop-home-con {
    height: 100vh;
    min-height: 100vh;
  }
  .main-page-wrap {
    width: 100%;
  }

</style>
