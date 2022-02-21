<template>
    <div class="con flex">
        <div class="con-inner flex">
            <img class="logo" @click="goHome" src="../images/logo.png" alt="">
            <div class="search-bar-con flex" v-if="!payment">
                <input type="text" v-model="search" name="search" placeholder="Search by product" @input="filteredUsers">
                <div class="search-img-con flex">
                    <img src="../images/search-1.png" alt="">
                </div>
            </div>
            <button class="trolley-con flex" v-if="!payment">
                <img src="../images/trolley.png" alt="" @click="bigBasket(this.basket)">
                <p class="basket-num">{{itemCount}}</p>
                <BasketDrop :basket="this.basket" @big-basket="bigBasket" @payment="$emit('payment')" @increase-amount="updateQuantity" @delete-item="deleteItem"/>
            </button>
        </div>
        
    </div>
</template>

<script>
import BasketDrop from './BasketDrop'

export default {
    name: 'Header',
    props: {
        basket: Object,
        payment: Boolean,
    },
    components: {
        BasketDrop,
    },
    data() {
        return {
            search: '',
        }
    },
    computed: {
        filteredUsers() {
            this.$emit('filter-products', this.search) 
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
        updateQuantity(newQ, p) {
            this.$emit('increase-amount', newQ, p)
        },
        deleteItem(item) {
            this.$emit('delete-item', item)
        },
        bigBasket(bask) {
            this.$emit('big-basket', bask)
        },
        goHome() {
            this.$emit('go-home')
        }
    }
}
</script>

<style scoped>
    .con {
        width: 100%;
        justify-content: center;
        background-color: #363636;
    }
    .con-inner {
        width: 100%;
        justify-content: flex-start;
        max-width: 1400px;
        padding: 40px;
    }
    .trolley-con {
        background-color: #646464;
        border: none;
        margin-left: auto;
        box-shadow: 0px 0px 6px #696969c9;
        border-radius: 10px;
        padding: 6px;
        position: relative;
    }
    .trolley-con:hover > .drop {
        display: flex;
    }
    .trolley-con img {
        width: 36px;
        margin-left: auto;
        transition: all 0.1s ease;
        cursor: pointer;
    }
    .trolley-con:hover > img {
        padding: 2px;
    }
    .logo {
        width: 60px;
        padding: 5px;
        cursor: pointer;
    }
    /* Search Bar */
    .search-bar-con {
        position: relative;
        width: 100%;
        max-width: 500px;
        margin-left: 40px;
    }
    .search-bar-con input {
        width: 100%;   
        padding: 8px;
        border-radius: 10px;
        border: 1px solid #949494c9;  
        padding-right: 48px;
        font-size: 14px;
        box-shadow: 0px 0px 6px #949494c9;
    }
    .search-img-con {
        position: absolute;
        right: 0px;
        padding-right: 10px;
        cursor: pointer;
        border-left: 1px solid #949494c9;
        height: 95%;
    }
    .search-img-con img {
        height: 30px;
        padding-left: 6px;
    }
    .search-bar-con input:focus {
        outline: none;
        border: 1px solid #424242c9;
    }
    .search-bar-con input:focus ~ .search-img-con {
        border-left: 1px solid #424242c9;
    }
    .basket-num {
        position: absolute;
        color: #ff3604;
        font-size: 24px;
        padding: 2px;
        top: 40%;
        left: 80%;
        background-color: #ffffff;
        line-height: 26px;
        border-radius: 10px;
        font-weight: 600;
    }

    /* Media queries */
    @media screen and (max-width: 780px) {
        .con-inner {
            padding: 20px;
        }
    }
    @media screen and (max-width: 780px) {
        .search-bar-con {
            width: unset;
        }
    }
    @media screen and (max-width: 400px) {
        .search-bar-con {
            margin-left: 0px;
        }
        .con-inner {
            padding: 10px;
        }
        .basket-num {
            left: unset;
            top: 70%;
            background-color: transparent;
        }
    }
    @media screen and (max-width: 340px) {
        .con-inner {
            flex-wrap: wrap;
        }
        .search-img-con img {
            height: 18px;
        }
        .search-bar-con input {
            padding: 4px;
        }
        .search-bar-con {
            order: 3;
            width: 100%;
            margin-top: 10px;
        }
        /* .logo {
            margin-left: auto;
        } */
        .trolley-con {
            order: 0;
        }
    @media screen and (max-width: 320px) {
        .trolley-con:hover > .drop {
            display: none;
        }
    }
    }
</style>