<template>
  <div class="container mx-auto">
    <exchange-rates />
  </div>
</template>

<script>
import ExchangeRates from '~/components/ExchangeRates.vue'

export default {
  components: {
    ExchangeRates
  }
}
</script>

<style>

.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
}

</style>
