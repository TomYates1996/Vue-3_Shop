<template>
    <div class="item-con flex-col">
        <img class="small-img" :src="require(`../images/${item.product.image}`)" alt="">
            <div class="quantity-box flex">
                <p>{{this.item.quantity}}</p>
            </div>        
    </div>
</template>

<script>
export default {
    name: 'BasketItem',
    props: {
        item: Object,
    },
    data() {
        return {
            quantity: this.item.quantity,
        }
    },
}
</script>

<style scoped>
    .item-con {
        background-color: #ffffff;
        margin: 5px 10px;
        justify-content: center;
        border-radius: 100%;
        max-width: 100px;
        position: relative;
    }
    .small-img {
        width: 80px;
        height: 80px;
        border-radius: 100%;
        cursor: pointer;
    }
    .quantity-box {
        position: absolute;
        bottom: 0px;
        right: -5px;
        color: #db0000;
    }
    .quantity-box p {
        font-size: 14px;
        font-weight: 600;
    }
    h4 {
        margin-right: auto;
        font-weight: 400;
        font-size: 14px;
    }
    .quantity-box input {
        border: 1px solid #b9b9b9ab;
        width: 18px;
        height: 18px;
        -webkit-appearance: textfield;
        -moz-appearance: textfield;
        appearance: textfield;
        text-align: right;
    }
    .quantity-box button {
        display: flex;
        align-items: center;
        justify-content: center;
        background-color: transparent;
        border: none;
        cursor: pointer;
    }
    .arrow {
        width: 10px;
    }
    .quantity-box button .arrow {
       height: 20px;
    }
    .price-div p {
        font-size: 12px;
    }
    .price-div {
        margin-right: 10px;
    }
    .total-price {
        font-size: 14px;
        margin-right: 10px;
        width: 50px;
        text-align: right;
    }
</style>