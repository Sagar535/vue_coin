<template>
  <div>
    Enter the name of the coin: <input type="text" name="coin_name" v-model="coin.name" v-on:keyup.13="getData">

    <p>Name: {{ coin.name }} </p>
    <p>Symbol: {{ coin.symbol }}</p>
    <p>Price (USD): {{ coin.price_usd }}</p>
    <p>Availability: {{ coin.available_supply }}</p>
  </div>
</template>

<script>
  import axios from 'axios'

  export default {
    name: 'Coins',

    data() {
      return {
        coin: {name: ''}
      }
    },

    created() {
      this.fetchData(),
        this.getData()
    },

    watch: {
      '$route': 'fetchData'
    },

    methods: {
      fetchData() {
        axios.get('https://api.coinmarketcap.com/v1/ticker/'+this.$route.params.id+'/')
          .then((resp) => {
            this.coin = resp.data[0];
            console.log(resp);
          })
          .catch((err) => {
            console.log(err);
          });
      },

      getData() {
        axios.get('https://api.coinmarketcap.com/v1/ticker/'+this.coin.name+'/')
          .then((resp) => {
            this.coin = resp.data[0];
            console.log(resp);
          })
          .catch((err) => {
            console.log(err);
          });
      }
    }
  }
</script>
