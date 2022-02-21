<template>
    <div class="item-con flex-col">
        <div class="check-head flex">
            <p>Item</p>
            <p>Price</p>
            <p>Qty</p>
            <p>Subtotal</p>
        </div>
        <FinalItem v-for="p in products" :key="p.id" :item="p" @delete-item="deleteItem" @update-quantity="updateQuantity"/>
    </div>
</template>

<script>
import FinalItem from './FinalItem'

export default {
    name: '',
    props: {
        products: Object,
    },
    components: {
        FinalItem,
    },
    emits: ['delete-item', 'increase-amount'],
    methods: {
        deleteItem(item) {
            this.$emit('delete-item', item)
        },
        updateQuantity(newQ, p) {
        this.$emit('increase-amount', newQ, p)
        }
    }
    
}
</script>

<style scoped>
    .item-con {
        width: 100%;
        background-color: #ffffff;
    }
    .check-head {
        width: 100%;
        justify-content: space-between;
        border-bottom: 1px solid #bebebe;
    }
    .check-head p {
        margin: 0px 20px;
        font-size: 14px;
        text-align: right;
    }
    .check-head p:first-of-type {
        margin-left: 120px;
    }
    .check-head p:nth-child(2) {
        margin-left: auto;
    }
    .check-head p:last-of-type {
        margin-right: 0px;
    }

    /* Media Queries */

    @media screen and (max-width: 445px) {
        .check-head p:first-of-type {
            margin-left: 96px;
        }
        .check-head p {
            margin-left: 0px;
        }
    }
    @media screen and (max-width: 350px) {
        .check-head p {
            font-size: 12px;
        }
    }
</style>