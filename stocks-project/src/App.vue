<template>
  <h1>Stock History App</h1>
  <div class="add-stock-wrapper">
    <input type="text" v-model="newStockInput" @keydown.enter="addStock">
    <button @click="addStock">Add Stock</button>
  </div>

  <div class="stock" v-for="stock in stocks" :key="stock.id">
    <span>{{ stock.name }}</span> <span class="delete"><button @click="removeStock(stock.id)">X</button></span>
  </div>
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'

export default {
  name: 'App',
  data() {
    return {
      newStockInput: "",
      stocks: []
    }
  },
  methods: {
    addStock() {
      let stockID = Math.floor(Math.random() * (9999999999 - 100000000) + 100000000)
      let newStock = {
        id: stockID,
        name: this.newStockInput
      }
      this.stocks.push(newStock)
      this.newStockInput = ""
    },

    removeStock(stockID) {
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
  max-width: 600px;
  margin: 0 auto;
}

button, input {
  border-radius: 7px;
  padding: 5px 10px;
  border: 1px solid #aaa;
  margin: 5px;
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
</style>
