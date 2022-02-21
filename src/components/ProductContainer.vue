<template>
    <div class="products-con flex-col">
        <div class="inner-product grid grid-con">
            <Product v-for="product in products" :key="product.id" :product="product" @add-to-basket="addToBasket"/>
        </div>
    </div>
</template>

<script>
import Product from './Product'

export default {
    name: 'ProductContainer',
    components: {
        Product,
    },
    props: {
        products: Object,
    },
    methods: {
        addToBasket(q, p) {
            this.$emit('add-to-basket', q, p)
        }
    }
}
</script>

<style scoped>
    .grid-con {
        grid-template-columns: repeat(4, 1fr);
        grid-template-rows: 400px;
    }
    .products-con {
        padding: 20px;
        margin: 20px;
        width: 100%;
    }
    .inner-product {
        max-width: 1100px;
        width: 100%;
    }
    /* Media */ 
    @media screen and (max-width: 1100px) {
        .grid-con {
            grid-template-columns: repeat(3, 1fr);
        }
        .inner-product {
            max-width: 800px;
            grid-template-rows: 320px minmax(0, 320px);
        }
    }
    @media screen and (max-width: 900px) {
        .inner-product {
            grid-template-rows: 300px minmax(0, 300px);
        }
    }
    @media screen and (max-width: 670px) {
        .products-con {
            padding: 0px;
        }
    }
    @media screen and (max-width:552px) {
        .inner-product {
            grid-template-rows: 310px minmax(0, 310px);
        }   
        .grid-con {
            grid-template-columns: repeat(2, 1fr);
        }
    }
    @media screen and (max-width: 340px) {
        .grid-con {
            grid-template-columns: 1fr;
        }
    }
</style>