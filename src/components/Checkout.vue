<template>
  <div class="checkout-con flex">
      <div class="checkout-inner flex-col">
          <h1>Basket</h1>
          <MyItems :products="basket" @delete-item="deleteItem" @increase-amount="increaseAmount"/>
          <div class="button-div flex">
                <button class="continue-shopping btno" @click="$emit('close')">Continue shopping</button>
                <button class="clear-cart btno" @click="$emit('clear')">Empty trolley</button>
          </div>
      </div>
      <div class="summary-bill flex-col">
          <h4>Summary</h4>
          <div class="stretch-p flex">
              <p>Subtotal</p>
              <p>£{{calculatePrice.toFixed(2)}}</p>
          </div>
          <div class="stretch-p flex">
              <p>Shipping <br> (Standard - 3-5 Days)</p>
              <p>£0.80</p>
          </div>
          <div class="stretch-p flex">
              <p>Order Total</p>
              <p>£{{(calculatePrice+0.8).toFixed(2)}}</p>
          </div>
          <button class="complete-purchase btno" @click="$emit('card-details')">Go to Checkout</button>
      </div>
  </div>
</template>

<script>
import MyItems from './MyItems'
import FinalBill from './FinalBill'

export default {
    name: 'Checkout',
    props: {
        basket: Object,
    },
    components: {
        MyItems,
        FinalBill,
    },
    emits: ['delete-item', 'close', 'clear', 'card-details', 'increase-amount'],
    methods: {
        deleteItem(item) {
            this.$emit('delete-item', item)
        },
        increaseAmount(newQ, p) {
            this.$emit('increase-amount', newQ, p)
        }
    },
    computed: {
        calculatePrice() {
            let price = 0;
            this.basket.forEach(item => {
                price = price + (item.product.price*item.quantity)
            });
            return price;
        }
    },  
}
</script>

<style scoped>
    .checkout-con {
        width: 100%;
        align-items: flex-start;
    }
    h1 {
        width: 100%;
        font-weight: 400;
    }
    .checkout-inner {
        width: 100%;
        max-width: 1100px;
        padding: 40px;
    }
    .button-div {
        width: 100%;
        justify-content: space-between;
        margin-top: 10px;
    }
    .btno:hover {
        background-color: #3d3d3d;
    }
    .btno {
        padding: 8px 20px;
        cursor: pointer;
        font-weight: 600;
        color: #ffffff;
        border: none;
        background-color: #ff7300;
    }
    .complete-purchase {
        width: 100%;
        padding: 14px;
        font-weight: 400;
        font-size: 15px;
        margin-top: 20px;
    }
    .summary-bill {
        background-color: #ebebeb;
        padding: 20px;
        margin: 5px;
        margin-top: 40px;
    }
    .summary-bill h4 {
        font-weight: 300;
        font-size: 26px;
        width: 100%;
        border-bottom: 1px solid #cacaca;
    }
    .stretch-p {
        font-size: 14px;
        font-weight: 300;
        justify-content: space-between;
        width: 100%;
        padding: 10px 0px;
        color: #3d3d3d;
    }
    .stretch-p:last-of-type p:last-of-type {
        font-weight: 500;
    }
    .stretch-p:last-of-type {
        border-bottom: 1px solid #cacaca;
    }

    /* Media Queries */

    @media screen and (max-width: 900px) {
        .btno {
            font-size: 12px;
        }
    }
    @media screen and (max-width: 650px) {
        .checkout-con {
            flex-direction: column;
        }
        .summary-bill {
            width: 100%;
            margin: 0px;
        }
        .checkout-inner {
            padding: 20px;
        }
    }
    @media screen and (max-width: 350px) {
        .btno {
            width: 50%;
            padding: 5px;
            font-weight: 300;
        }
        .clear-cart {
            margin-left: 2px;
        }
        .continue-shopping {
            margin-right: 2px;
        }
        .checkout-inner {
            padding: 10px;
        }
        .complete-purchase {
            width: 100%;
        }
    }
</style>