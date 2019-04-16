<template>
<v-container>
  <v-layout>
    <v-flex>
      <v-data-table
        :items="sortPrices"
        class="elevation-1"
        hide-actions
        hide-headers
      >
        <template v-slot:items="props">
          <td class="text-xs-left"><img style="width: 30px" :src="source + props.item.CoinInfo.ImageUrl" alt=""></td>
          <td class="text-xs-left">{{ props.item.CoinInfo.FullName }}</td>
          <td class="text-xs-left">{{ props.item.CoinInfo.Name }}</td>
          <td class="text-xs-left">{{ props.item.RAW.USD.PRICE }}</td>
          <td class="text-xs-left">{{ props.item.RAW.USD.SUPPLY }}</td>
        </template>
      </v-data-table>
    </v-flex>
  </v-layout>
</v-container>
</template>

<script>
import axios from 'axios'

  export default {
    data () {
      return {
        crypto: [],
        source: 'https://www.cryptocompare.com'
      }
    },
    computed: {
      sortPrices() {
        return [...this.crypto].sort((a, b) => b.RAW.USD.PRICE - a.RAW.USD.PRICE)
      }
    },
    mounted() {
      axios
        .get('https://min-api.cryptocompare.com/data/top/mktcapfull?limit=20&tsym=USD')
        .then(response => (this.crypto = response.data.Data))
      setInterval(() => {
      axios
        .get('https://min-api.cryptocompare.com/data/top/mktcapfull?limit=20&tsym=USD')
        .then(response => (this.crypto = response.data.Data))
      }, 300000)
    }
  }
</script>
