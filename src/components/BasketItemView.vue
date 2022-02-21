<template>
    <div class="outer flex">
        <img :src="require(`../images/${item.product.image}`)" alt="">
        <div class="content flex-col">
            <p>{{item.product.name}}</p>
            <p>£{{item.product.price.toFixed(2)}}</p>
            <div class="edit flex">
                <p>Qty: </p>
                <input v-model="this.quantity" @keypress="isNumber($event)" @change="updateQuantity" >
                <button class="delete-btn" @click="deleteItem"><img src="../images/bin.png" alt=""></button>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'BasketItemView',
    props: {
        item: Object,
    },
    data() {
        return {
            quantity: this.item.quantity
        }
    },
    watch: {
        item: {
            immediate: true,
            deep: true,
            handler(newValue, oldValue) {
                this.quantity = parseInt(newValue.quantity)
            }
        }
    },
    methods: {
         isNumber(evt) {
            evt = (evt) ? evt : window.event;
            var charCode = (evt.which) ? evt.which : evt.keyCode;
            if ((charCode > 31 && (charCode < 48 || charCode > 57)) && charCode !== 46) {
                evt.preventDefault();;
            } else {
                return true;
            }
        },
        updateQuantity() {
            this.quantity = parseInt(this.quantity);
            if (this.quantity <= 0) {
                this.deleteItem()
            } else {
                 this.$emit('update-quantity', this.quantity, this.item.product)
            }           
        },
        deleteItem() {
            this.$emit('delete-item', this.item)
        },
    }
}
</script>

<style scoped>
    .outer {
        align-items: flex-start;
        width: 100%;
        justify-content: flex-start;
        border-bottom: 1px solid #9b9b9b21;
        padding: 20px 0px;
    }
    .outer img {
        width: 50px;
        height: 50px;
    }
    .content {
        align-items: flex-start;
        margin-left: 10px;
        width: 100%;
    }
    .content input {
        width: 36px;
        padding: 2px;
        text-align: center;
        margin-left: 20px;
        margin-right: auto;
    }
    .edit {
        width: 100%;
    }
    .delete-btn {
        background-color: transparent;
        border: none;
        display: flex;
        justify-content: center;
        align-items: center;
        cursor: pointer;
    }
    .delete-btn img {
        width: 20px;
        height: 20px;
    }
</style>