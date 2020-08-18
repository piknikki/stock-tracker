<template>
  <div class="container-fluid margins">
    <div class="row">
      <div class="col-10">
        <input type="text"
               autofocus placeholder="Enter symbol..." v-model="symbol" v-on:keyup.enter="search"/>
      </div>
      <div class="col-2">
        <input type="button" value="Go" v-on:click="search" />
      </div>
    </div>

    <stock-quote v-bind:stock="stock"></stock-quote>
    <div class="alert alert-danger" role="alert" v-if="error">
      <strong>Error!</strong> {{this.error}}
    </div>
  </div>
</template>

<!--<template>-->
<!--  <div id="app">-->
<!--    <div id="header">-->
<!--      <h1>Header</h1>-->
<!--      <span class="green-items">${{ currentPrice }} / {{ percentage }}%</span>-->
<!--    </div>-->

<!--    <ul class="items" v-for="item in items" :key="item">-->
<!--      <li>-->
<!--        <span>{{ item.name }}</span>-->
<!--        <span class="price">{{ item.price }}</span>-->
<!--      </li>-->
<!--    </ul>-->
<!--    <div>-->
<!--      <input type="text"-->
<!--      placeholder="Enter symbol "-->
<!--      v-model="symbol"-->
<!--      >-->
<!--      <button>Search</button>-->
<!--    </div>-->
<!--  </div>-->
<!--</template>-->

<script>
// @ is an alias to /src
import StockQuote from '@/components/StockQuote'

export default {
  data () {
    return {
      stock: {},
      error: '',
      symbol: ''
    }
    // return {
    //   items: [
    //     {
    //       name: 'name 1',
    //       price: 20.00,
    //       currentPrice: 3.99,
    //       percentage: 2.5
    //     },
    //     {
    //       name: 'name 2',
    //       price: 30.00,
    //       currentPrice: 3.99,
    //       percentage: 6.5
    //     }
    //   ]
    // }
  },
  components: {
    'stock-quote': StockQuote
  },
  methods: {
    init () {
      this.stock = {}
      this.error = ''
    },
    search () {
      const axios = require('axios')

      axios({
        method: 'GET',
        url: 'https://investors-exchange-iex-trading.p.rapidapi.com/stock/' + this.symbol + '/book',
        headers: {
          'content-type': 'application/octet-stream',
          'x-rapidapi-host': 'investors-exchange-iex-trading.p.rapidapi.com',
          'x-rapidapi-key': '6c9880c44emsh99301f9d46cb5ebp11fbebjsn7fcfd3f93a9e',
          useQueryString: true
        }
      })
        .then((response) => {
          this.stock = response.data.quote
          console.log(response.data.quote)
        })
        .catch(this.handleErrors)
    },
    handleErrors (err) {
      if (err.status === 404) {
        this.error = ' That symbol could not be found. Please use another.'
      } else {
        this.stock = ''
        this.error = ' There was an error retrieving the data. Please try again.'
        console.log(err.response)
      }
    }
  }
}
</script>

<style>
ul, li {
  list-style-type: none;
  list-style-position:inside;
  margin:0;
  padding:0;
}

ul {
  display: flex;
  flex-direction: column;
}

li {
  display: flex;
  flex-direction: row;
  flex-wrap: nowrap;
  border-bottom: 1px solid black;
  margin-bottom: 10px;
  padding: 20px;
  font-size: 1.5em;
}

.price {
  margin-left: auto;
}

#header {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
}

.green-items {
  font-size: 2em;
  color: green;
}
</style>
