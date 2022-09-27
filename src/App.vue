<!-- eslint-disable vue/no-unused-vars -->
<!-- eslint-disable vue/require-v-for-key -->

<template>
  <div class="container">
    <div class="row">
      <h1>Crypto Market</h1>
      <input
        type="text"
        class="form-control bg-light text-dark rounded-0 border-0 my-4"
        placeholder="Search Coin"
        @keyup="searchCoin()"
        v-model="textSearch"
      />
      <table class="table table-light">
        <thead>
          <tr>
            <th v-for="data in information" :key="data">
              {{ data }}
            </th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(coin, index) in filteredCoins" :key="coin.id">
            <td class="text-mute">
              {{ index + 1 }}
            </td>
            <td>
              <img
                :src="coin.image"
                alt="Not found"
                style="width: 2rem"
                class="me-2"
              />
              <span class="me-2">
                {{ coin.name }}
              </span>
              <span class="text-uppercase text-muted">
                {{ coin.symbol }}
              </span>
            </td>
            <td>$ {{ coin.current_price }}</td>
            <td
              :class="[
                coin.price_change_percentage_24h > 0
                  ? 'text-success'
                  : 'text-danger',
              ]"
            >
              {{ coin.price_change_percentage_24h }} %
            </td>
            <td>$ {{ coin.total_volume.toLocaleString() }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "App",
  data() {
    return {
      coins: [],
      filteredCoins: [],
      information: ["#", "Coin", "Price", "Price Graphic", "24hs Volume"],
      textSearch: "",
    };
  },
  async mounted() {
    const response = await axios.get(
      "https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&order=market_cap_desc&per_page=100&page=1&sparkline=false"
    );
    console.log(response.data);
    this.coins = response.data;
    this.filteredCoins = response.data;
  },
  methods: {
    searchCoin() {
      this.filteredCoins = this.coins.filter(
        (coin) =>
          coin.name.toLowerCase().includes(this.textSearch.toLowerCase()) ||
          coin.symbol.toLowerCase().includes(this.textSearch.toLowerCase())
      );
    },
  },
};
</script>

<style>
</style>
