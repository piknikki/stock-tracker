<template>
  <div id="app">
    <div id="header">
      <h1>Header</h1>
      <span class="green-items">${{ currentPrice }} / {{ percentage }}%</span>
    </div>

    <ul class="items" v-for="item in items" :key="item">
      <li>
        <span>{{ item.name }}</span>
        <span class="price">{{ item.price }}</span>
      </li>
    </ul>
  </div>
</template>

<script>
// @ is an alias to /src

export default {
  name: 'Home',
  data () {
    return {

      items: [
        {
          name: 'name 1',
          price: 20.00,
          currentPrice: 3.99,
          percentage: 2.5
        },
        {
          name: 'name 2',
          price: 30.00,
          currentPrice: 3.99,
          percentage: 6.5
        }
      ]
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
