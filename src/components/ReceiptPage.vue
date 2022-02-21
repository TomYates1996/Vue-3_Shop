<template>
  <div class="receipt-con flex-col">
    <h3>Order Confirmation</h3>
    <p>Order number: 4567842968942</p>
    <p>An order confirmation will be sent to {{details.ship.email}} </p>
    <p>Your parcel will be delivered in {{details.shippingMethod.time}} </p>
    <p>Thank you for your custom</p>
    <div class="main-box flex-col">
      <div class="details-box flex">
        <div class="shipping-details-box flex-col col-box">
          <h4>Shipping Address</h4>
          <p>{{details.ship.firstName}} {{details.ship.lastName}}</p>
          <p>{{details.ship.streetAddress}}</p>
          <p>{{details.ship.town}}</p>
          <p>{{details.ship.postcode}}</p>
          <p>{{details.ship.country}}</p>
          <p>{{details.ship.phone}}</p>
        </div>

        <div class="billing-details-box flex-col col-box">
          <h4>Billing Address</h4>
          <p>{{details.bill.firstName}} {{details.ship.lastName}}</p>
          <p>{{details.bill.streetAddress}}</p>
          <p>{{details.bill.town}}</p>
          <p>{{details.bill.postcode}}</p>
          <p>{{details.bill.country}}</p>
          <p>{{details.bill.phone}}</p>
        </div>

        <div class="card-details-box flex-col col-box">
          <h4>Card Details</h4>
          <p>Name on card: {{details.bill.firstName}} {{details.ship.lastName}}</p>
          <p>Card Number: {{details.card.card}}</p>
          <p>Expiry Date: {{details.card.expMon}}/{{details.card.expYr}}</p>
          <p>CVV: {{details.card.cvn}}</p>
        </div>
      </div>
      <div class="order-content flex-col">
        <h4>Final Bill</h4>
        <div class="oc-line flex">
          <p>Shipping Price</p>
          <p>{{details.shippingMethod.price.toFixed(2)}}</p>
        </div>
        <div class="oc-line flex">
          <p>Products Price</p>
          <p>{{((calculatePrice/100)*80).toFixed(2)}}</p>
        </div>
        <div class="oc-line flex">
          <p>Taxes</p>
          <p>{{((calculatePrice/100)*20).toFixed(2)}}</p>
        </div>
        <div class="oc-line discount flex" v-if="details.discount.isDiscount">
          <p>Discount {{details.discount.amount}}</p>
          <p>{{((calculatePrice*(details.discount.amount/100))).toFixed(2)}}</p>
        </div>
        <div class="oc-line flex">
          <p>Total Paid</p>
          <p><strong>{{((calculatePrice*((100-details.discount.amount)/100))+details.shippingMethod.price).toFixed(2)}}</strong></p>
        </div>
      </div>
    </div>
    
  </div>
</template>

<script>
export default {
    name: 'ReceiptPage',
    props: {
      details: Object,
      basket: Object,
    },
    computed: {
        calculatePrice() {
            let price = 0;
            this.details.basket.forEach(item => {
                price = price + (item.product.price*item.quantity)
            });
            return price;
        }
    },  
}
</script>

<style scoped>
  .receipt-con {
    width: 100%;
  }
  .receipt-con h3 {
    margin: 10px 0px;
  }
  .main-box {
    width: 100%;
  }
  .details-box {
    width: 100%;
    align-items: flex-start;
    height: 160px;
    margin-bottom: 20px;
  }
  .col-box {
    width: 100%;
    background-color: #ebebeb;
    margin: 10px;
    align-items: flex-start;
    padding: 10px;
    height: 100%;
  }
  .col-box h4 {
    width: 100%;
    text-align: center;
  }
  .col-box p {
    font-size: 14px;
    line-height: 18px;
  }
  .order-content {
    background-color: #ff7300;
    padding: 10px 20px;
    font-weight: 500;
    width: 100%;
    align-items: flex-end;
  }
  .order-content h4 {
    margin-bottom: 4px;
    border-bottom: 1px solid #000000;
    width: 100%;
    text-align: center;
    max-width: 500px;
  }
  .oc-line {
    justify-content: space-between;
    width: 100%;
    max-width: 500px;
  }
  .oc-line p:last-of-type {
    margin-left: 20px;
  }
  .discount {
    border-top: 1px solid #696969;
    border-bottom: 1px solid #696969;
  }
  /* Media Queries */

  @media screen and (max-width: 800px) {
    .order-content {
      align-items: center;
    }
    .details-box {
      display: grid;
      grid-template-columns: repeat(2, 1fr);
      height: unset;
      gap: 10px;
      padding: 10px;
      margin-bottom: 0px;
    }
    .col-box {
      margin: 0px;
    }
  }
  @media screen and (max-width: 650px) {
    .details-box {
      grid-template-columns: 1fr;
      padding: 0px;
    }
    .main-box {
      flex-direction: row;
      padding: 10px;
    }
    .order-content {
      height: 100%;
    }
    .oc-line p {
      font-size: 12px;
    }
  }
</style>