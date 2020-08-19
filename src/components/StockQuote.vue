<template>
  <div>
    <div id="stock-card" class="quote" v-if="stock.symbol">
      <div>
        <h1 id="company-name">{{ stock.companyName }}</h1>
      </div>
      <div class="flex-row">
        <span class="left header">{{ stock.latestPrice }}</span>
        <span class="right header" :style="style">{{stock.change}} ({{stock.changePercent}}%)</span>
      </div>
      <div class="flex-row bottom">
        <span class="left">Range</span>
        <span class="right">{{stock.low}} - {{stock.high}}</span>
      </div>
      <div class="flex-row bottom">
        <span class="left">Open</span>
        <span class="right">{{stock.open}}</span>
      </div>
      <div class="flex-row bottom">
        <span class="left">Volume</span>
        <span v-bind:formatNumber="formatNumber" class="right">{{stock.latestVolume}}</span>
      </div>
      <div class="flex-row bottom">
        <span class="left">Market Cap</span>
        <span class="right">{{stock.marketCap}}</span>
      </div>
      <div class="flex-row bottom">

        <span class="right time">As of {{stock.latestTime}}</span>
      </div>
    </div>
  </div>
</template>

<script>
var numeral = require('numeral')

export default {
  props: ['stock', 'color'],
  methods: {
    formatNumber (value) {
      return numeral(value).format('0.0a')
    }
  },
  computed: {
    style () {
      if (this.stock.change > 0) {
        return 'color: green'
      } else if (this.stock.change < 0) {
        return 'color: red'
      } else {
        return 'color: black'
      }
    }
  }
}
</script>

<style scoped>
#company-name {
  width: 40%;
  text-align: left;
  margin-left: 20px;
  letter-spacing: 0.1em;
}

#company-name, .header {
  text-transform: uppercase;
  display: flex;
  justify-items: left;
  flex-wrap: wrap;
  font-family: 'Lora', serif;
}

#stock-card {
  display: flex;
  flex-direction: column;
}

.flex-row {
  display: flex;
  flex-direction: row;
  border-bottom: 1px solid gray;
  padding: 20px;
  font-size: 1em;
}

.bottom {
  font-family: 'Roboto', sans-serif;
}

.left {
  font-weight: light;
}

.right {
  margin-left: auto;
  font-weight: bold;
}

.time {
  font-weight: light;
}

.header {
  font-size: 2em;
  font-weight: bold;
}

</style>
