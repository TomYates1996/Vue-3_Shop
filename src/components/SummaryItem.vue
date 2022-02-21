<template>
  <div class="box-con flex-col">
      <div class="box-top flex">
          <img class="thumbnail" :src="require(`../images/${item.product.image}`)" alt="">
          <div class="inner-box flex-col">
              <div class="name-price flex">
                <p>{{item.product.name}}</p>
                <div class="price-box flex-col">
                    <p> £{{(item.product.price*item.quantity).toFixed(2)}}</p>
                    <p> £{{((item.product.price*item.quantity)*((100-discount)/100)).toFixed(2)}}</p>
                </div>
                
              </div>
              <div class="quantity-inc flex">
                    <p>Qty:</p>
                    <input type="number" v-model="quantity" min="0" oninput="validity.valid||(value='');">
                    <div class="stacked-btns flex-col">
                        <button @click="changeAmount(1, $event)"><img class="arrow" src="../images/up.png" alt=""></button>
                        <button @click="changeAmount(-1, $event)"><img class="arrow" src="../images/down.png" alt=""></button>
                    </div>
                    <button class="bin" @click="deleteItem"><img src="../images/bin.png" alt=""></button>
              </div>
          </div>
      </div>
  </div>
</template>

<script>
export default {
    name: 'SummaryItem',
    props: {
        item: Object,
        discount: Number,
    },
    data() {
        return {
            quantity: this.item.quantity
        }
    },
    methods: {
        changeAmount(n, e) {
            e.preventDefault()
            if (this.quantity + n < 0) {
                return
            } else {
                this.quantity = this.quantity + n
            }
            if (this.quantity == 0) {
                this.$emit('delete-item', this.item)
            } else {
                this.$emit('new-quantity', this.item, this.quantity)
            }            
        },
        deleteItem() {
            this.$emit('delete-item', this.item)
        }
    }
}
</script>

<style scoped>
    .box-con {
        width: 100%;
        padding: 10px 0px;
    }
    .box-top {
        width: 100%;
    }
    .thumbnail {
        width: 70px;
        height: 70px;
        margin-right: 10px;
    }
    .quantity-inc input {
        border: 1px solid #b9b9b9ab;
        width: 36px;
        height: 30px;
        -webkit-appearance: textfield;
        -moz-appearance: textfield;
        appearance: textfield;
        text-align: center;
    }
    .stacked-btns button {
        height: 14px;
        width: 14px;
        justify-content: center;
        display: flex;
        align-items: center;
        cursor: pointer;
        border: none;
    }
    .arrow {
        width: 14px;
        height: 14px;
    }
    .inner-box {
        width: 100%;
        align-items: flex-start;   
    }
    .inner-box p {
        width: 100%;
        font-size: 15px;
    }
    .name-price {
        width: 100%;
        justify-content: space-between;
    }
    
    .bin img {
        width: 30px;
    }
    .bin {
        background-color: transparent;
        border: none;
        cursor: pointer;
        margin-left: 20px;
    }
    /* Media Queries */
    @media screen and (max-width: 600px) {
        .bin img {
            width: 20px;
        }
    }
</style>