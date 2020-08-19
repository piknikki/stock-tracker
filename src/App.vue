<template id="app">
  <div id="container">
    <div class="row">
      <hr>
      <div class="col-10">
        <stock-quote v-if="stock" v-bind:stock="stock"></stock-quote>
        <div class="alert alert-danger" role="alert" v-if="error">
          <strong>Error!</strong> {{this.error}}
        </div>
      </div>
      <div class="button-area">
        <input type="text" class="buttons"
               autofocus placeholder="Enter symbol..." v-model="symbol" v-on:keyup.enter="search"/>
        <span>
          <input class="buttons" type="button" value="Get New Quote" v-on:click="search" />

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
      var url = `https://sandbox.iexapis.com/stable/stock/${this.symbol}/quote?token=Tpk_186b74daec6147939cdcd9bf6638450e`

      axios.get(url)
        .then((response) => {
          this.stock = response.data
          console.log(response.data)
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

<style lang="scss">
#app {
  font-family: 'Roboto', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

.logo {
  width: 20px;
  padding-right: 10px;
}

#nav {
  padding: 30px;

  a {
    font-weight: bold;
    font-size: 1.2em;
    color: #2c3e50;
    text-decoration: none;
    justify-content: center;
    margin: 20px;

    &.router-link-exact-active {
      color: #14A9BF;

    }
  }
}

hr {
  border-top: 2px solid gray;
}

input, button {
  padding: 0.2em;
  margin: 0.1em 0.2em;
  box-sizing: content-box;
  -moz-box-sizing: content-box;
  -webkit-box-sizing: content-box;
  font-family: 'Cabin', sans-serif;
}

#container {
  width: 40%;
  margin: 100px auto;
}

.buttons {
  width: 40%;
  margin: 10px;
  padding: 10px;
  font-size: 1em;
}
</style>
