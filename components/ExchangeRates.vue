<template>
  <div class="exchange-rates-container border-t-2 border-teal-600">
    <div class="grey1 text-gray-600 font-medium py-2 px-3 uppercase">Exchange Rates</div>

    <table class="w-full">
      <thead>
        <tr class="grey2 text-gray-600 border-b-2 border-gray-400">
          <th class=""></th>
          <th v-for="(currency, index) of mainCurrencies" :key="index" class="px-4 py-2 font-normal">
            <div class="flex">
              <currency-icon class="mr-2" :currency="currency" />
              {{ currency }}              
            </div>
          </th>
          <th class="px-4 py-2"></th>
        </tr>
      </thead>
      <tbody>
        <template v-for="exchange of exchanges">
          <tr class="grey3 text-gray-600 text-xs font-bold">
            <td :colspan="mainCurrencies.length + 1" class="px-4 py-2">{{ transfromDate(exchange.startAt) }} - {{ transfromDate(exchange.endAt) }}</td>
            <td class="px-4 py-2">
              <svg class="fill-current h-3 w-3 text-black" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg"><path d="M10 12a2 2 0 100-4 2 2 0 000 4zm0-6a2 2 0 100-4 2 2 0 000 4zm0 12a2 2 0 100-4 2 2 0 000 4z"/></svg>
            </td>
          </tr>
          <tr class="blue1 text-sm border-t border-gray" v-for="currency of mainCurrencies">
            <td class="px-4 py-2">
              <div class="flex text-xs items-center">
                <currency-icon class="mr-2" :currency="currency" />
                1 {{ currency }} corresponds to
              </div>
            </td>
            <td v-for="subCurrency of mainCurrencies" class="px-4 py-2">
              {{ getRate(exchange, currency, subCurrency ) }}
            </td>
            <td class="px-4 py-2">
              <svg class="fill-current h-3 w-3" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg"><path d="M10 12a2 2 0 100-4 2 2 0 000 4zm0-6a2 2 0 100-4 2 2 0 000 4zm0 12a2 2 0 100-4 2 2 0 000 4z"/></svg>
            </td>
          </tr>          
        </template>

      </tbody>
    </table>
    <div class="bg-gray-300 text-gray-600 text-sm py-2 text-center">
      <div>add time period</div>
    </div>

  </div>
</template>

<script>
import CurrencyIcon from '~/components/CurrencyIcon.vue'

export default {
  components: {
    CurrencyIcon
  },
  methods: {
    transfromDate(date) {
      date = new Date(date)
      return `${date.getUTCDay() + 1}.${date.getUTCMonth() + 1}.${date.getFullYear()}`
    },
    getRate(exchange, currency, subCurrency) {
      if(currency === subCurrency) {
        return "-"
      }
      let rate = exchange.rates.find(r => {
        return (r.fromCurrencyCode === currency && r.toCurrencyCode === subCurrency)
      })

      if(rate !== undefined) {
        return rate.rate
      }

      return "-"
    }
  },
  mounted() {
    this.$axios.$options('').then(res => {
      this.mainCurrencies = res.mainCurrencies

      this.$axios.$get('').then(res => {
        this.exchanges = res
      })
    })
  },
  data() {
    return {
      mainCurrencies: [],
      exchanges: []
    }
  }
}
</script>

<style lang="scss">
  .exchange-rates-container {
    width: 520px;
  }

  .grey1 {
    background-color: #F7F9FA;
  }

  .grey2 {
    background-color: #EAEAEA;
  }

  .grey3 {
    background-color: #EEEEEE;
  }
  
  .blue1 {
    background-color: rgb(232, 244, 255);
  }


</style>