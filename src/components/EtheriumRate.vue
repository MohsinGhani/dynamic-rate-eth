<template>
    <div id="app">
      <h1>Ethereum</h1>
      <p>Price in BTC: {{ priceInBTC }}</p>
      <p>Price in USD: {{ priceInUSD }}</p>
      
      <select v-model="selectedLanguage" @change="fetchDescription">
        <option value="en">English</option>
        <option value="ja">Japanese</option>
        <option value="ru">Russian</option>
      </select>
  
      <div v-html="ethereumDescription"></div>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  
  export default {
    data() {
      return {
        priceInBTC: '',
        priceInUSD: '',
        selectedLanguage: 'en',
        ethereumDescription: '',
      };
    },
    methods: {
      fetchEthereumData() {
        axios.get('https://api.coingecko.com/api/v3/coins/ethereum')
          .then(response => {
            this.priceInBTC = response.data.market_data.current_price.btc;
            this.priceInUSD = response.data.market_data.current_price.usd;
            this.ethereumDescription = response.data.description[this.selectedLanguage];
          })
          .catch(error => console.error(error));
      },
      fetchDescription() {
        this.fetchEthereumData();
      }
    },
    mounted() {
      this.fetchEthereumData();
      setInterval(this.fetchEthereumData, 40000);
    }
  };
  </script>

  