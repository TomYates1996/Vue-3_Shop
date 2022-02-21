<template>
    <div class="colu flex-col">
        <div class="summary-con flex-col">
            <h3>Order Summary</h3>
            <div class="price-box flex-col">
                <div class="subs flex">
                    <p>Cart Subtotal</p>
                    <!-- <p>£{{this.price}}</p> -->
                    <p>£{{calculatePrice.toFixed(2)}}</p>
                </div>
                <div class="subs discount flex">
                    <p>Discount - {{this.discountAmount}}%</p>
                    <div class="discount-inner flex-col">
                        <p>£{{(calculatePrice*(discountAmount/100)).toFixed(2)}}</p>
                        <p>£{{(calculatePrice-(calculatePrice*(discountAmount/100))).toFixed(2)}}</p>
                    </div>
                    
                </div>
                <div class="subs flex">
                    <div class="shipping-ps flex-col">
                        <p>Shipping</p>
                        <p>{{this.shipping.name}} - {{this.shipping.time}}</p>
                    </div>
                    
                    <p>£{{shipping.price.toFixed(2)}}</p>
                </div>
                <div class="subs flex">
                    <p>Order Total</p>
                    <p><strong> £{{(calculatePrice*((100-this.discountAmount)/100) + this.shipping.price).toFixed(2)}}</strong></p>
                </div>
            </div>
            <h3>{{itemCount}} items in Cart</h3>
            <SummaryItem v-for="product in basket" :discount="this.discountAmount " :key="product.id" :item="product" @delete-item="deleteItem" @new-quantity="newQuantity"/>
        </div>
        <div class="gift-voucher flex">
            <input type="text" name="voucher" v-model="voucher" placeholder="Enter gift voucher">
            <button class="apply-voucher" @click="discount">Apply Voucher</button>
        </div>
        <div class="get-emails flex">
            <input type="checkbox" checked>
            <h5>Recieve offers & news by email?</h5>
        </div>
    </div>   
</template>

<script>
import SummaryItem from './SummaryItem'

export default {
    name: 'FinalBill',
    components: {
        SummaryItem,
    },
    props: {
        basket: Object,
        shipping: Object,
    },
    data() {
        return {
            price: 0,
            orderDetails: [],
            voucher: '',
            discounted: false,
            discountAmount: 0,
        }
    },
    emits: ['discount'],
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
    methods: {
        deleteItem(item) {
            this.$emit('delete-item', item)
        },
        newQuantity(item, quantity) {
            this.basket[this.basket.indexOf(item)].quantity = quantity;
        },
        discount(e) {
            e.preventDefault()
            if (this.voucher.toLowerCase() == '20percent') {
                document.querySelector('.discount').style.display = 'flex';
                this.discounted = true;
                this.discountAmount = 20;
                document.querySelectorAll('.box-con .price-box p:last-of-type').forEach(el => {
                    el.style.display = 'flex';
                })
            }
            this.$emit('discount-send', {isDiscount: this.discounted, amount: this.discountAmount})
        }
    }     
}
</script>

<style scoped>
    .colu {
        width: 100%;
    }
    .summary-con {
        background-color: #ebebeb;
        padding: 20px;
        width: 100%;
    }
    .colu h3 {
        border-bottom: 1px solid #a0a0a0;
        width: 100%;
        font-size: 28px;
        font-weight: 300;
    }
    .price-box {
        width: 100%;
    }
    .discount {
        display: none;
    }
    .discount-inner p:last-of-type {
        color: #ff7300;
        border-bottom: 1px solid #696969;
        border-top: 1px solid #696969;
    }
    .price-box p {
        font-size: 15px;
        font-weight: 300;
        padding: 4px 0px;
    }
    .subs {
        width: 100%;
        justify-content: space-between;
    }
    .shipping-ps {
        align-items: flex-start;
    }
    .shipping-ps p:last-of-type {
        color: #919191;
    }
    .gift-voucher {
        margin: 20px 0px;
    }
    .gift-voucher input {
        width: 100%;
        margin-right: 10px;
        width: 100%;
        padding: 4px;
        border: 1px solid #a0a0a057;
        color: #696969;
        padding-left: 10px;
        font-size: 14px;
    }
    .gift-voucher input:focus {
        color: #000000;
        outline: none;
        border: 1px solid #38383857;
    }
    .apply-voucher {
        width: 200px;
        padding: 5px;
        border: none;
        background-color: #353535;
        color: #ffffff;
        font-weight: 400;
        transition: all 0.3s ease;
        cursor: pointer;
    }
    .apply-voucher:hover {
        background-color: #ff7300;
    }
    .get-emails {
        width: 100%;
        justify-content: flex-start;
        margin-bottom: 20px;
    }
    .get-emails input {
        margin-right: 10px;
        cursor: pointer;
    }
    .get-emails h5 {
        font-size: 15px;
        font-weight: 500;
    }
    .place-order {
        width: 100%;
        background-color: #ff7300;
        border: none;
        color: #ffffff;
        padding: 14px;
        font-size: 15px;
        font-weight: 500;
        cursor: pointer;
    }

    /deep/ .box-con .price-box p:last-of-type::before {
        content: '';
        width: 120%;
        left: -10%;
        height: 1px;
        background-color: #ff7300;
        position: absolute;
        top: -50%;
    }
    /deep/ .box-con .price-box p:last-of-type {
        position: relative;
        color: #ff7300;
        border-top: 1px solid #696969;
        display: none;
    }

    @media screen and (max-width: 1100px) { 
        .gift-voucher {
            width: 100%;
        }
    }
    @media screen and (max-width: 300px) {
        .gift-voucher {
            padding: 0px 5px;
            flex-wrap: wrap;
        }
        .apply-voucher {
            margin-top: 10px;
            width: 100%;
            padding: 10px;
        }
        .get-emails {
            padding: 0px 5px;
        }
    }
</style>