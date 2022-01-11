<template>
  <section class="order-finder">

    <v-date-picker class="order-finder__date-picker" v-model="chosenDay" @change="getOrders(chosenDay)" />

    <div class="order-finder__item-wrapper">

      <h3 v-if="guestQueryError">Error fetching orders.</h3>
      <h3 v-else-if="guestQueryComplete && !guestData.length">No orders on this day.</h3>

      <template v-else-if="guestQueryComplete">
        <h3>This day's orders</h3>
        <div class="order-finder__sub-wrapper">
          <OrderItem
            v-for="guest in guestData"
            :key="guest.id"
            :guest="guest" />
        </div>
      </template>
      
      <h3 v-else class="order-finder__heading">Please select a date</h3>
    </div>

  </section>
</template>

<script>
  import axios from 'axios'
  import OrderItem from '../components/OrderItem'

  export default {
    name: 'OrderFinder',

    components: {
      OrderItem
    },
    
    data() {
      return {
        guestData: [],
        chosenDay: '2019-07-21',
        guestQueryComplete: false,
        guestQueryError: false
      }
    },

    methods: {
      getOrders(chosenDay) {

        const headers = {'Access-Control-Allow-Origin': `*`,
                         'X-FIB-API-LANGUAGE': `en_GB`,
                         'X-FIB-API-AUTH-TYPE': `exam`,
                         'X-FIB-API-AUTH-TOKEN': `F6HCAFVHPEg3"Sw#`}

        axios.get(`https://tst-api.feeditback.com/exam.guests?limit=100&since=${chosenDay} 00:00&until=${chosenDay} 23:59`, { headers })
          .then(response => {
            this.guestData = response.data.items
            this.guestQueryComplete = true
          })
          .catch(error => {
            this.guestQueryComplete = false
            this.guestQueryError = true
            console.log(error)
          })
      }
    }
  }
</script>

<style lang="scss" scoped>

  .order-finder {
    display: flex;
    flex-direction: column;

    @media screen and (min-width: 760px) {
      flex-direction: row;
      justify-content: center;
    }

    &__date-picker {
      @media screen and (min-width: 760px) {
        margin-right: 32px;
      }
    }
    
    &__item-wrapper {
      flex-grow: 1;
      max-width: 600px;
    }

    &__sub-wrapper {
      display: flex;
      flex-direction: column;
    }

    &__heading {
      color: red;
    }
  }

</style>