<template>
    <div class="item-bar-con flex-col">
        <div class="main-in flex">
            <img class="image" :src="require(`../images/${item.product.image}`)" alt="">
            <h3>{{item.product.name}}</h3>
            <div class="right-inner flex">
                <p>£{{item.product.price.toFixed(2)}}</p>
                <input v-model="quantity" @keypress="isNumber($event)" @change="updateQuantity" >
                <!-- <p class="quantity-text">({{item.quantity}})</p> -->
                <p class="price-text">£{{(item.quantity*item.product.price).toFixed(2)}}</p>
            </div>
           
        </div>
        <button class="delete-item" @click="deleteItem">Remove item</button>
    </div>
</template>

<script>
export default {
    name: 'FinalItem',
    props: {
        item: Object,
    },
    data() {
        return {
            quantity: this.item.quantity,
        }
    },
    emits: ['delete-item', 'update-quantity'],
    methods: {
        deleteItem() {
            this.$emit('delete-item', this.item)
        },
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
        }
    }
}
</script>

<style scoped>
    .item-bar-con {
        width: 100%;
        justify-content: flex-start;
        border-bottom: 1px solid #bebebe;
        align-items: flex-start;
        padding: 10px 0px;
    }
    .main-in {
        width: 100%;
    }
    .image {
        width: 80px;
        padding: 3px;
        height: 80px;
    }
    h3 {
        margin-left: 40px;
        font-weight: 300;
        font-size: 14px;
    }
    .right-inner {
        margin-left: auto;
    }
    .right-inner p {
        font-size: 14px;
    }
    .right-inner input {
        width: 50px;
        font-size: 14px;
        line-height: 20px;
        margin: 0px 36px;
        text-align: center;
    }
    .price-text {
        margin-left: auto;
        margin-right: 3px;
    }
    .quantity-text {
        margin-left: 10px;
    }
    .delete-item {
        background-color: #3d3d3d;
        padding: 4px 10px;
        color: #ffffff;
        font-weight: 500;
        font-size: 14px;
        border: none;
        margin-top: 20px;
        cursor: pointer;
        transition: all 0.2s ease;
    }
    .delete-item:hover {
        background-color: #ff7300;
    }
     /* Media Queries */
    @media screen and (max-width: 900px) {
        .delete-item {
            font-size: 12px;
            margin-top: 10px;
        }
    }
    @media screen and (max-width: 445px) {
        h3 {
            margin-left: 14px;
        }
        .right-inner input {
            margin: 0px 14px;
        }
        .main-in img {
            width: 50px;
            height: 50px;
        }
    }
    @media screen and (max-width: 350px) {
        .right-inner p {
            font-size: 12px;
        }
        h3 {
            font-size: 12px;
        }
        .delete-item {
            font-weight: 300;
            margin-top: 4px;
        }
    }

</style>