<template>

  <div class="order-item">
    <div class="order-item__header">
      <p class="order-item__guest-info">{{ guest.first_name }} {{ guest.last_name }}<span class="order-item__sub-info">({{ guest.gender }}, {{ guest.email }})</span></p>
      <p class="order-item__visit-date">Visited: {{ guest.visited }}</p>
    </div>

    <p v-if="orderQueryError">Error fetching order details</p>

    <div v-else class="order-item__order-wrapper">
      <table>
        <tr>
          <th>Item</th>
          <th>QTY</th>
          <th>Price</th>
        </tr>
        <tr v-for="order in orderData" :key="order.id">
          <td>{{ order.name }}</td>
          <td>{{ order.quantity }}</td>
          <td>£{{ order.price }}</td>
        </tr>
      </table>
      <p v-if="orderData.length" class="order-item__order-total">Total spend: <strong>£{{ totalSpend() }}</strong></p>
    </div>
  </div>

</template>

<script>
  import axios from 'axios';

  export default {
    name: 'OrderItem',
    
    data() {
      return {
        orderData: [],
        orderQueryError: false
      }
    },

    props: {
      guest: {
        type: Object,
        required: true
      }
    },

    mounted() {
      this.fetchOrderDetails()
    },

    methods: {

      fetchOrderDetails() {

        const headers = {'Access-Control-Allow-Origin': `*`,
                         'X-FIB-API-LANGUAGE': `en_GB`,
                         'X-FIB-API-AUTH-TYPE': `exam`,
                         'X-FIB-API-AUTH-TOKEN': `F6HCAFVHPEg3"Sw#`}

        axios.get(`https://tst-api.feeditback.com/exam.guests.orders?limit=100&guest_id=${this.guest.id}`, { headers })
          .then(response => {
            this.orderData = response.data.items
          })
          .catch(error => {
            this.orderQueryError = true
            console.log(error)
          })
          
      },

      totalSpend() {
        var totalSpend = this.orderData.reduce(function (accumulator, order) {
          return accumulator + order.price;
        }, 0);

        return totalSpend
      }
    }
  }

</script>

<style lang="scss" scoped>

  .order-item {
    margin-top: 16px;
    border-radius: 4px;
    border: 1px solid #e8e8e8;
    overflow: hidden;

    &__header {
      padding: 12px;
      background-color: lightcoral;
    }

    &__guest-info {
      margin: 0;
      font-size: 1.4em;
    }

    &__sub-info {
      margin: auto 0 auto 8px;
      font-size: 0.6em;
    }

    &__visit-date {
      margin: 0;
      font-size: 0.8em;
    }

    &__order-wrapper {
      padding: 12px;
    }

    &__order-total {
      margin: 8px 8px 0 0;
      text-align: right;
    }

    table {
      border-spacing: 0px;
    }

    td {
      min-width: 48px;
      border-bottom: 1px solid #efefef;
      text-align: center;

      &:first-child {
        width: 100%;
        text-align: left;
      }
    }

    tr:last-child td {
      border-bottom: 2px solid black;
    }
  }

</style>