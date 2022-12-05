<template>
  <h1>Stock History App</h1>
  <div class="add-stock-wrapper">
    <input type="text" v-model="newStockInput" @keydown.enter="addStock">
    <input type="date" v-model="defaultDate">
    <button @click="addStock">Add Stock</button>
  </div>

  <div class="stock" v-for="stock in stocks" :key="stock.id">
    <div class="stock-line">
    <!-- <div> -->
      <div class="stock-name">{{ stock.stock_name }}</div>
      <div>{{ stock.date }}</div>
      <hr>
      <!-- <div>{{ stock.results }}</div> -->
      <div> {{"Opening Price: "}} {{ stock.o }}</div>
      <div> {{"Closing Price: "}} {{ stock.c }}</div>
      <div> {{"Highest Price: "}} {{ stock.h }}</div>
      <div> {{"Lowest Price: "}} {{ stock.l }}</div>
      <div> {{"Volume traded: "}} {{ stock.v }}</div>
      <span class="delete"><button @click="removeStock(stock.id)">X</button></span>
    </div>
    <!-- <div class="details-line">
      <span>{{ stock.results}}</span>
    </div> -->
  </div>
  <!-- <span>{{ stocks }}</span> -->
  <!-- <span>{{ "hello" }}</span> -->
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'

export default {
  name: 'App',
  data() {
    return {
      newStockInput: "",
      stocks: [],
      defaultDate: '2022-11-29'
    }
  },
  async created () {
    const response = await fetch("https://68bu66vaxf.execute-api.us-east-1.amazonaws.com/stocks")
    this.stocks = await response.json()
    this.stocks = this.stocks.sort((a, b) => a.name - b.name );
  },
  methods: {
    async addStock() {
      let stockID = Math.floor(Math.random() * (9999999999 - 100000000) + 100000000)
      let newStock = {
        id: stockID,
        stock_name: this.newStockInput,
        date: this.defaultDate 
      }

      await fetch("https://68bu66vaxf.execute-api.us-east-1.amazonaws.com/stocks", {
        method: "post",
        headers: {
          "Content-Type": "application/json"
        },
        body: JSON.stringify(newStock)
      })

      this.stocks.push(newStock)
      this.newStockInput = ""
      window.location.reload();
    },

    async removeStock(stockID) {
       
      await fetch("https://68bu66vaxf.execute-api.us-east-1.amazonaws.com/stocks", {
        method: "delete",
        headers: {
          "Content-Type": "application/json"
        },
        body: JSON.stringify({id: stockID})
      })

      this.stocks = this.stocks.filter(s => s.id !== stockID)
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  max-width: 450px;
  margin: 0 auto;
}

button, input {
  border-radius: 7px;
  padding: 5px 10px;
  border: 1px solid #aaa;
  margin: 5px;
}

button {
  font-size: large;
}

.add-stock-wrapper {
  display: flex;
}

.add-stock-wrapper input {
  flex: 1;
}


.stock {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background-color: #eee;
  border-radius: 10px;
  margin: 5px 10px;
  padding: 10px 10px;
}

.stock-line {
  flex: 1;
}

.stock-name {
  font-size: x-large;
}


</style>
