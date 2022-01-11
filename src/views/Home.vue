<template>
  
  <section class="home">
    <h1 class="home__title">Restaurant App Test</h1>

    <OrderFinder />

    <p class="home__author-tag">App by Nathan</p>
  </section>

</template>

<script>
  import OrderFinder from '../components/OrderFinder'

  export default {
    name: 'Home',

    components: {
      OrderFinder,
    },
  }
</script>

<style lang="scss" scoped>

  .home {
    min-height: 100vh;
    padding: 32px 16px;
    display: flex;
    flex-direction: column;

    @media screen and (min-width: 760px) {
      padding: 32px 48px;
    }

    &__title {
      margin-bottom: 24px;
      font-size: 2.5em;
      text-align: center;
    }

    &__author-tag {
      margin: auto 0 0;
      padding-top: 24px;
      font-size: 0.8em;
      font-style: italic;
      text-align: center;
    }
  }

</style>