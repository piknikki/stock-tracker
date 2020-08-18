<template>
  <div id="container">
    <div class="row">
      <div class="col-10">
        <stock-quote v-if="stock" v-bind:stock="stock"></stock-quote>
        <div class="alert alert-danger" role="alert" v-if="error">
          <strong>Error!</strong> {{this.error}}
        </div>
      </div>
      <div class="button-area">
        <input type="text"
               autofocus placeholder="Enter symbol..." v-model="symbol" v-on:keyup.enter="search"/>
        <span>
          <input class="go-button" type="button" value="Go" v-on:click="search" />
        </span>
      </div>
    </div>
  </div>
</template>

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

      this.init()
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

#container {
  width: 50%;
  margin: 0 auto;
}

input {
  margin: 10px 30px;
}

.go-button {
  width: 100px;
}
</style>
