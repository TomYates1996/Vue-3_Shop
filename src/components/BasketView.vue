<template>
    <div class="outer-con flex-col">
        <BasketItemView v-for="item in basket" :key="item.id" :item="item" @delete-item="deleteItem" @update-quantity="updateQuantity"/>
    </div>
</template>

<script>
import BasketItemView from './BasketItemView'

export default {
    name: 'BasketView',
    props: {
        basket: Object,
    },
    components: {
        BasketItemView,
    },
    methods: {
        updateQuantity(newQ, p) {
            this.$emit('increase-amount', newQ, p)
        },
        deleteItem(item) {
            this.$emit('delete-item', item)
        },
    }
}
</script>

<style scoped>
    .outer-con {
        width: 100%;
        padding: 0px 20px;
        border-top: 1px solid #9b9b9b21;
        border-bottom: 1px solid #9b9b9b21;
        overflow-y: scroll;
        max-height: 250px;
    }
</style>