<template>
  <div class="drop flex-col">
      <div class="drop-head flex-col">
          <div class="drop-text flex">
            <h4><strong>{{itemCount}}</strong> Items in Basket</h4>
            <div class="flex-col headend">
                <h4>Basket Subtotal:</h4>
                <h3>£{{calculatePrice.toFixed(2)}}</h3>
            </div>
          </div>
          <button class="checkout-btn" @click="$emit('payment')">Go to Checkout</button>
      </div>
      <BasketView :basket="this.basket" @increase-amount="updateQuantity" @delete-item="deleteItem" />
      <div class="drop-foot flex">
          <button class="view-basket" @click="basketBig">View and Edit Basket</button>
      </div>
  </div>
</template>

<script>
import BasketView from './BasketView'

export default {
    name: 'BasketDrop',
    props: {
        basket: Object,
    },
    components: {
        BasketView,
    },
    methods: {
        updateQuantity(newQ, p) {
            this.$emit('increase-amount', newQ, p)
        },
        deleteItem(item) {
            this.$emit('delete-item', item)
        },
        basketBig() {
            this.$emit('big-basket', this.basket)
        }
    },
    computed: {
        calculatePrice() {
            let price = 0;
            this.basket.forEach(item => {
                price = price + (item.product.price*item.quantity)
            });
            return price;
        },
        itemCount() {
            let count = 0
            this.basket.forEach(el => {
                count = count + el.quantity
            })
            return count
        }
    },  
}
</script>

<style scoped>
    .drop {
        position: absolute;
        top: 100%;
        right: 0px;
        background-color: #ffffff;
        box-shadow: 0px 0px 6px #75757586;
        width: 400px;
        display: none;
        z-index: 15;
    }
    .drop-head {
        width: 100%;
        padding: 20px;
    }
    .drop-text {
        width: 100%;
        justify-content: space-between;
        align-items: flex-start;
        margin-bottom: 20px;
    }
    .drop-text h4 {
        font-weight: 300;
        font-size: 14px;
        color: #3b3b3b;
    }
    .headend {
        align-items: flex-end;
    }
    .drop-text h3 {
        font-weight: 600;
    }
    .checkout-btn {
        width: 100%;
        background-color: #ff7300;
        border: none;
        color: #ffffff;
        padding: 14px;
        font-size: 15px;
        font-weight: 500;
        cursor: pointer;
        transition: all 0.1s ease;
    }
    .checkout-btn:hover {
        background-color: #3b3b3b;
    }
    .drop-foot {
        padding: 20px;
    }
    .view-basket {
        background-color: #3b3b3b;
        border: none;
        color: #ffffff;
        padding: 14px;
        font-size: 14px;
        font-weight: 500;
        cursor: pointer;
        transition: all 0.1s ease;
    }
    .view-basket:hover {
        background-color: #ff7300;
    }

    /* Media Queries */

    @media screen and (max-width: 900px) {
        .drop {
            width: 300px;
        }
    }
    @media screen and (max-width: 340px) {
        .drop {
            z-index: 10;
        }
    }
</style>