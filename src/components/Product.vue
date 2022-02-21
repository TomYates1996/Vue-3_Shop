<template>
    <div class="product-con flex-col">
        <img class="product-img" :src="require(`../images/${product.image}`)" alt="">
        <div class="product-inner flex-col">
            <h2>{{product.name}}</h2>
            <p class="price">£{{product.price.toFixed(2)}} per fruit</p>
            <p>Get it Tomorrow, {{dateTomorrow}}</p>
            <div class="quantity-box flex">
                <select class="quantity-select" id="quantity" @change="changeQuantity($event)">
                    <option value="" selected disabled>{{ this.buyQuantity }}</option>
                    <option v-for="x in 30" :value="x" :key="x">{{ x }}</option>
                </select>
            </div>
            <div class="star-box flex">
                <div class="star-inner">
                    <img class="stars-hollow" src="../images/stars-outline.png" alt="">
                    <div class="background-fill" :style="`width: ${this.rating}%`"></div>
                </div>
                <p>{{product.raters}}</p>
            </div>
            <button class="add-to-basket" @click="addToBasket">Add to basket</button>
        </div>
        
        
    </div>
  
</template>

<script>
export default {
    name: 'Product',
    props: {
        product: Object,
    }, 
    data() {
        return {
            buyQuantity: 1,
            rating: ~~ this.product.rating
        }
    },
    computed: {
        dateTomorrow() {
            let today = new Date();
            let tomorrow = new Date(today);
            tomorrow.setDate(tomorrow.getDate() + 1);
            let halfTom = tomorrow.toString().slice(0,10)
            return halfTom;
        }
    },
    methods: {
        changeAmount(n) {
            if (this.buyQuantity + n < 1) {
                return
            } else {
                this.buyQuantity = this.buyQuantity + n
            }
        },
        addToBasket() {
            this.$emit('add-to-basket', this.buyQuantity, this.product);
            // alert(`${this.buyQuantity} ${this.product.name} added to basket`)   
            // this.buyQuantity = 1;       
        },
        changeQuantity(event) {
            this.buyQuantity = parseInt(event.target.options[event.target.options.selectedIndex].text)
            console.log(this.buyQuantity);
        },
    },
}
</script>

<style scoped>
    .product-con {
        width: 100%;
        transition: box-shadow 0.3s ease;
        justify-content: flex-start;
        padding: 10px 20px;
        height: 400px;
        background-color: #ffffff;
        border-bottom: 1px solid #a8a8a89a;
    }
    .product-con:hover .star-box {
        display: flex;
    }
    .product-con:hover {
        height: 420px;
        z-index: 4;
        box-shadow: 0px 0px 10px #b9b9b9ab;
        border-bottom: none;
    }
    .star-box {
        display: none;
        overflow: hidden;
        margin: 0px 0px 10px 0px;
    }
    .product-inner {
        width: 100%;
        align-items: center;
    }
    .product-inner p:first-of-type {
        font-size: 16px;
        text-align: center;
    }
    .product-inner p:last-of-type {
        font-size: 12px;
        text-align: center;
    }
    .product-img {
        width: 200px;
        height: 200px;
        border-radius: 10px;
    }
    .product-con h2 {
        font-size: 22px;
        font-weight: 400;
    }
    .price {
        margin-right: 20px;
    }
    button {
        background-color: transparent;
        border: none;
        cursor: pointer;

    }
    .add-to-basket {
        background-color: #ff7300;
        padding: 10px;
        color: #ffffff;
    }
    .add-to-basket:hover {
        background-color: #555555;
    }

    .quantity-box input {
        border: 1px solid #b9b9b9ab;
    }
    .quantity-box button {
        display: flex;
        align-items: center;
        justify-content: center;
    }
    .quantity-box button .arrow {
       height: 20px;
    }
    #quantity {
        margin-bottom: 4px;
    }
    .star-inner {
        position: relative;
        height: 20px;
    }
    .stars-hollow {
        width: 100px;
        z-index: 3;
        position: relative;
        height: 20px;
    }
    .background-fill {
        background-color: #ffd000;
        position: absolute;
        height: 100%;
        top: 0px;
    }

    /* Media Queries */
    @media screen and (max-width: 1100px) {
        .product-con {
            height: 320px
        }
        .product-con:hover {
            height: 340px;
        }
        .product-img {
            width: 130px;
            height: 130px;
        }
    }
    @media screen and (max-width: 900px) {
        .product-img {
            width: 110px;
            height: 110px;
        }
        .product-con {
            height: 300px
        }
        .product-con:hover {
            height: 320px;
        }
    }
    @media screen and (max-width: 670px) {
        .product-img {
            width: 130px;
            /* height: 130px; */
        }
        .product-con {
            padding: 5px 10px;
        }
        .product-con:hover {
            height: 320px;
        }
    }
    @media screen and (max-width: 600px) {
        .product-inner h2 {
            font-size: 18px;
        }
        .product-inner p:first-of-type {
            font-size: 12px;
        }
    }
    @media screen and (max-width:552px) {
        .product-con {
            height: 310px;
        }
        .product-con:hover {
            height: inherit;
        }
        .star-box {
            display: flex;
        }
    }
</style>