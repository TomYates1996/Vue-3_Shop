<template>
  <div class="form-con flex">
      <form @submit="onSubmit" class="form-inner flex">
          <div class="colu card flex-col">
              <h3>Shipping Address</h3>
              <label for="email" class="name-label">Email Address</label>
              <input type="email" id="email" name="email" v-model="email" placeholder="" required>
              <label for="first-name" class="name-label">First Name</label>
              <input type="text" id="first-name" name="firstName" v-model="firstName" placeholder="" required>
              <label for="last-name" class="name-label">Last Name</label>
              <input type="text" id="last-name" name="lastName" v-model="lastName" placeholder="" required>
              <label for="street-address" class="name-label">Street Address</label>
              <input type="text" id="street-address" name="street-address" v-model="streetAddress" placeholder="" required>
              <label for="town-city" class="name-label">Town/City</label>
              <input type="text" id="town-city" name="town-ciy" v-model="townCity" placeholder="" required>
              <label for="postcode" class="name-label">Postcode/Zip</label>
              <input type="text" id="postcode" name="postcode" v-model="postcode" :maxlength="8" placeholder="" required>
              <label for="country" class="name-label">Country</label>
              <select class="form-control" @change="changeCountry($event)" required>
                <option value="" selected disabled>Select</option>
                <option v-for="country in countries" :value="country.id" :key="country.id">{{ country.name }}</option>
              </select>
              <label for="phone-number" class="name-label" >Phone Number</label>
              <input type="text" id="phone-number" name="phone" v-model="phone" :maxlength="12" @keypress="isNumber($event)" required>
          </div>


          <div class="colu unmarge flex-col">
              <div class="bill-card card flex-col">
                  <h3>Shipping Methods</h3>
                  <div class="ship-option flex">
                      <input type="radio" class="shipping-choice" name="shipping-choice" @click="setShipping({price: 0.8, name: 'Standard', time: '3-5 Days'})" checked>
                      <p>£0.80</p>
                      <p>3-5 Days</p>
                      <p>Standard</p>
                  </div>
                  <div class="ship-option flex">
                      <input type="radio" class="shipping-choice" name="shipping-choice" @click="setShipping({price: 2, name: 'Next Day', time: '1 Day'})">
                      <p>£2.00</p>
                      <p>1 Day</p>
                      <p>Next Day</p>
                  </div>
              </div>


              <div class="card-col card flex-col">
                  <h3>Payment Method</h3>
                  <div class="bill-ship flex">
                        <label for="same-bill"><input id="same-bill" class="same-ship-bill" type="checkbox" @change="toggleAddress" checked>My billing and shipping address are the same</label>
                  </div>           
                  <div class="bill-address flex-col">
                    <label for="first-nameb" class="name-label">First Name</label>
                    <input type="text" id="first-nameb" name="firstName" v-model="firstNameBill" placeholder="">
                    <label for="last-nameb" class="name-label">Last Name</label>
                    <input type="text" id="last-nameb" name="lastName" v-model="lastNameBill" placeholder="" >
                    <label for="street-addressb" class="name-label">Street Address</label>
                    <input type="text" id="street-addressb" name="street-address" v-model="streetAddressBill" placeholder="" >
                    <label for="town-cityb" class="name-label">Town/City</label>
                    <input type="text" id="town-cityb" name="town-ciy" v-model="townCityBill" placeholder="">
                    <label for="postcodeb" class="name-label">Postcode/Zip</label>
                    <input type="text" id="postcodeb" name="postcode" v-model="postcodeBill" placeholder="" >
                    <label for="countryb" class="name-label">Country</label>
                    <select class="form-control" id="countryb" @change="changeCountryBill($event)">
                        <option value="" selected disabled>Select</option>
                        <option v-for="country in countries" :value="country.id" :key="country.id">{{ country.name }}</option>
                    </select>
                    <label for="phone-numberb" class="name-label">Phone Number</label>
                    <input type="text" id="phone-numberb" name="phone" v-model="phoneBill"  :maxlength="12" @keypress="isNumber($event)">
                  </div>     
                  <div class="pay-opt flex">
                      <input type="radio" name="payment-choice" class="paypal-radio" checked>
                      <img src="../images/paypal.png" alt="">
                      <p>PayPal</p>
                      <button class="paypal-button">Pay With <strong style="font-size: 16px;">PayPal</strong></button>
                  </div>
                  <div class="nobord pay-opt flex-col">
                      <input type="radio" name="payment-choice">
                      <p class="floating-p">Card Payment</p>
                      <div class="card-details-form flex-col">
                          <div class="card-number c-i flex">
                              <p>Card Number</p>
                              <input type="text" name="card-number" v-model="card" :maxlength="16" @keypress="isNumber($event)" required>
                          </div>
                          <div class="expiry c-i flex">
                              <p>Expiration Date</p>
                              <input type="text" name="expiry-mon" id="" placeholder="MM" :maxlength="2" @keypress="isNumber($event)" v-model="expMon" required>
                              <p>/</p>
                              <input type="text" name="expiry-year" placeholder="YY" :maxlength="2" @keypress="isNumber($event)" v-model="expYr" required>
                          </div>
                          <div class="cvn c-i flex">
                              <p>CVV</p>
                              <input type="text" name="cvn" v-model="cvn" :maxlength="3" required>
                          </div>
                      </div>
                  </div>
              </div>
          </div>
          <div class="right-col flex-col">
            <FinalBill :basket="this.basket" :shipping="this.shipping" @delete-item="deleteItem" @discount-send="discountSet"/>
            <input type="submit" value="Place Order" class="place-order">
          </div>
      </form>
  </div>
</template>

<script>
import FinalBill from './FinalBill'

export default {
    name: 'PaymentPage',
    components: {
        FinalBill,
    },
    props: {
        basket: Object,
    },
    data() {
        return {
            countries: [
                {name: 'Argentina'},
                {name: 'Brazil'},
                {name: 'Chile'},
                {name: 'Denmark'},
                {name: 'Ethiopia'},
                {name: 'France'},
                {name: 'Germany'},
                {name: 'Hungary'},
                {name: 'India'},
                {name: 'Japan'},
                {name: 'Kyrgyzstan'},
                {name: 'Laos'},
                {name: 'Mauritius'},
                {name: 'Niger'},
                {name: 'Oman'},
                {name: 'Peru'},
                {name: 'Qatar'},
                {name: 'Rwanda'},
                {name: 'Sweden'},
                {name: 'Timor'},
                {name: 'United Kingdom'},
                {name: 'United States'},
                {name: 'Venezuela'},
            ],
            country: '',
            email: '',
            firstName: '',
            lastName: '',
            streetAddress: '',
            townCity: '',
            postcode: '',
            phone: '',
            voucher: '',
            countryBill: '',
            firstNameBill: '',
            lastNameBill: '',
            streetAddressBill: '',
            townCityBill: '',
            postcodeBill: '',
            phoneBill: '',
            card: '',
            cvn: '',
            expMon: '',
            expYr: '',
            shipping: {price: 0.8, name: 'Standard', time: '3-5 Days'},
            sharedAddress: true,
            discount: {isDiscount: false, amount: 0}
        }
    },
    methods: {
        changeCountry(event) {
            this.country = event.target.options[event.target.options.selectedIndex].text
        },
        changeCountryBill(event) {
            this.countryBill = event.target.options[event.target.options.selectedIndex].text
        },
        deleteItem(item) {
            this.$emit('delete-item', item)
        },
        toggleAddress() {
            let check = document.querySelector('.same-ship-bill');
            if (check.checked == true) {
                document.querySelector('.bill-address').style.display = 'none';
                document.querySelectorAll('.bill-address input').forEach(el => {
                    el.required = false;
                })
                this.sharedAddress = true;
            } else {
               document.querySelector('.bill-address').style.display = 'flex';
               document.querySelectorAll('.bill-address input').forEach(el => {
                    el.required = true;
                })
                this.sharedAddress = false;
            }
        },
        setShipping(a) {
            this.shipping = a;
        },
        discountSet(d) {
            this.discount = d;
        },
        onSubmit(e) {
            e.preventDefault();
            let ship = {
                email: this.email,
                firstName: this.firstName,
                lastName: this.lastName,
                streetAddress: this.streetAddress,
                town: this.townCity,
                postcode: this.postcode,
                country: this.country,
                phone: this.phone,
            }
            let bill = {}
            if (this.sharedAddress == true) {
                bill = {
                firstName: this.firstName,
                lastName: this.lastName,
                streetAddress: this.streetAddress,
                town: this.townCity,
                postcode: this.postcode,
                country: this.country,
                phone: this.phone
                }
            } else {
                bill = {
                    firstName: this.firstNameBill,
                    lastName: this.lastNameBill,
                    streetAddress: this.streetAddressBill,
                    town: this.townCityBill,
                    postcode: this.postcodeBill,
                    country: this.countryBill,
                    phone: this.phoneBill
                }
            }
            let card = {
                card: this.card,
                cvn: this.cvn,
                expMon: this.expMon,
                expYr: this.expYr,
            }
            let shopping = this.basket
            let buyerDetails = {
                ship: ship,
                bill: bill,
                card: card,
                shippingMethod: this.shipping,
                basket: shopping,
                discount: this.discount
            }
            console.log(buyerDetails.discount);
            this.$emit('place-order', buyerDetails) 
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
    }
}
</script>

<style scoped>
    .form-con {
        background-color: #f8f8f8;
        width: 100%;
        align-items: flex-start;
    }
    .form-inner {
        width: 100%;
        align-items: flex-start;
        max-width: 1400px;
    }
    .checkbox-con {
        position: relative;
        height: 100%;
        width: 30px;
    }
    .checkbox-fake {
        background-color: #a0a0a0;
        height: 16px;
        width: 16px;
        border-radius: 100%;
        position: absolute;
        left: 0px;
        z-index: 1;
        top: calc(50% - 8px);
    }
    .paypal input:checked ~ .checkbox-fake {
        background-color: #3600cc;
    }
    .card {
        padding: 20px;
        background-color: #ffffff;
        width: 100%;
        box-shadow: 0px 0px 2px #49494967;
        transition: all 0.3s ease;
        margin: 12px 0px;
    }
    .card:hover {
        box-shadow: 0px 0px 14px #49494967;
    }
    .card h3 {
        border-bottom: 1px solid #a0a0a0;
        width: 100%;
        font-size: 28px;
        font-weight: 300;
    }
    .ship-option {
        width: 100%;
        justify-content: space-between;
        margin-top: 10px;
    }
    .colu {
        margin: 12px 12px 12px 12px;
        width: 100%;
    }
    .unmarge {
        margin-top: 0px;
    }
    .colu input {
        width: 100%;
        padding: 4px;
        border: 1px solid #a0a0a057;
        color: #696969;
    }
    .colu label {
        text-align: left;
        width: 100%;
        font-size: 14px;
        font-weight: 300;
        margin: 4px 0px;
    }
    .colu select {
        width: 100%;
        padding: 4px;
        border: 1px solid #a0a0a057;
        color: #696969;
    }
    .colu select:focus {
        border: 1px solid #38383857;
    }
    .colu input:focus {
        color: #000000;
        outline: none;
        border: 1px solid #38383857;
    }
    .bill-ship {
        justify-content: flex-start;
        padding: 10px 0px;
        width: 100%;
    }
    .bill-ship input {
        width: 20px;
    }
    .bill-ship label {
        font-size: 14px;
    }
    .pay-opt {
        width: 100%;
        justify-content: flex-start;
        padding: 10px 0px;
        border-bottom: 1px solid #a0a0a0;
        flex-wrap: wrap;
    }
    .paypal-button {
        width: 100%;
        margin-top: 10px;
        padding: 4px;
        border: none;
        background-color: #2a94f8;
        color: #ffffff;
        font-size: 12px;
        border-radius: 5px;
        display: none;
        justify-content: center;
    }
    .paypal-radio:checked ~ .paypal-button {
        display: flex;
    }
    .pay-opt input {
        width: auto;
        margin-right: 10px;
        cursor: pointer;
    }
    .pay-opt p {
        font-weight: 300;
        font-size: 14px;
    }
    .pay-opt img {
        width: 70px;
        border: 1px solid #acacac;
        margin-right: 10px;
    }
    .nobord {
        border: none;
        align-items: flex-start;
        position: relative;
        padding: 14px 0px;
    }
    .floating-p {
        position: absolute;
        left: 24px;
        top: 10px;
    }
    .card-payment {
        width: 100%;
    }
    .card-details-form {
        display: none;
        align-items: flex-start;
    }
    .card-details-form input {
        width: 100%;
        text-align: right;
    }
    .nobord input:checked ~ .card-details-form {
        display: flex;
    }
    .expiry input,
    .cvn input {
        width: 50px;
        margin-left: 10px;
    }
    .c-i {
        margin: 5px 0px;
    }
    .bill-address {
        display: none;
        width: 100%;
    }
    .colu .shipping-choice {
        width: auto;
    }
    .ship-option p {
        font-weight: 300;
        widows: 100%;
    }
    .ship-option p:first-of-type {
        margin-right: auto;
        margin-left: 10px;
    }
    .ship-option p:last-of-type {
        margin-left: 20px;
    }
    .shipping-choice {
        cursor: pointer;
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
        margin-bottom: 10px;
    }
    .right-col {
        padding-right: 20px;
        margin-left: 12px;
        min-width: 500px;
    }
    /* Media Queries */
    @media screen and (max-width: 1250px) {
        .right-col {
            min-width: 400px;
        }
    }
    @media screen and (max-width: 1100px) {
        .form-inner {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            padding: 40px;
            gap: 20px;
        }
        .right-col {
            grid-column: 1/3;
            margin-left: 0px;
            padding-right: 0px;
        }
        .colu {
            margin: 0px;
        }
        .card {
            margin: 0px;
        }
        .bill-card {
            margin-bottom: 20px;
        }
    }
    @media screen and (max-width: 780px) {
        .form-inner {
            padding: 10px;
            gap: 10px;
        }
        .bill-card {
            margin-bottom: 10px;
        }
    }
    @media screen and (max-width: 662px) {
        .colu {
            grid-column: 1/3;
        }
    }
    @media screen and (max-width: 500px) {
        .form-inner {
            padding: 5px;
            gap: 5px;
        }
        .bill-card {
            margin-bottom: 5px;
        }
         .right-col {
            min-width: unset;
        }
    }
    @media screen and (max-width: 300px) {
        .form-inner {
            padding: 0px;
        }
        .card {
            padding: 10px;
        }
        .place-order {

        }
    }

</style>

