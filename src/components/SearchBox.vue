<template>
  <section class="form">
    <div class="field">
      <label class="label">Search for a car brand</label>
      <div class="input">
        <input type="text" v-model='query' @input='lookupQuery' @change="saveQuery" title="Start typing a car brand" placeholder="Search..." :key="componentKey"/>
        <div class="button" @click="saveQuery" title="Execute search"><i class="fas fa-search"></i></div>
      </div>
      <div class="hits" v-bind:class="{'open': hitList}">
        <ul class="dropdown-menu">
          <li v-for='brands in matches'	v-bind:key="brands" v-bind:title="'Select ' + brands">
            <a v-bind:href="brands">{{ brands }}</a>
          </li>
        </ul>
      </div>
      <div class="history" v-if="history.length >= 1">
        <div class="history_header">
          <div class="history_header_text">Search history</div>
          <div class="button" @click="clearHistory">Clear</div>
        </div>
        <ul>
          <li v-for="savedQuery in history" :disabled="!savedQuery" v-bind:key="savedQuery.id" v-bind:value="savedQuery.query" >
            <div class="history_query">{{ savedQuery.query }}</div>
            <div class="history_timestamp">{{ savedQuery.timestamp }}</div>
            <div class="button" v-bind:value="savedQuery.id" @click="deleteQuery"><i class="fas fa-times"></i></div>
          </li>
        </ul>
      </div>
    </div>
  </section>
</template>
  
<script>
  import brands from "./brands"
  export default {
    data() {
      return {
        open: false,
        componentKey: 0,
        query: "",
        history: [],
        brands: brands,
        form: "Search",
      }
    },
    name: "SearchBox",
    computed: {
      matches() {
        let hit = ""
        return this.brands.filter((hit) => {
          return hit.toLowerCase().indexOf(this.query) >= 0
        })
      },
      hitList() {
        return this.query != "" && this.matches.length != 0 && this.open == true
      }
    },
    methods: {
      saveQuery(event){
        this.open = false
        const query = event.target.value
        const timestamp = new Date().toLocaleString()
        if (this.history.length <= 0)
          this.history[0] = { id: 0, query: query, timestamp: timestamp }
        else {
          const newId = parseInt(this.history[this.history.length-1].id) + 1
          this.history[newId] = { id: newId, query: query, timestamp: timestamp}
        }
        this.componentKey++
      },
      deleteQuery(event){
        this.open = false
        this.history.splice(event.target.value, 1)
      },
      clearHistory(){
        this.open = false
        this.history = []
      },
      lookupQuery() {
        !this.open ? this.open = true : this.open
      }
    },
  }
</script>

<style>
  .form{
    position: relative;
    width: 100%;
  }
  .label{
    font-weight: bold;
    color: black;
  }
  .input{
    width: 100%;
    border: 1px solid lightgray;
    font-size: 1.2em;
  }
  input{
    width: 90%;
    border: none;
    padding-left: .5em;
  }
  .history{
    width: 100%;
    background-color: lightgray;
  }
  .history_header{
    width: 100%;
    font-size: .7em;
    background-color:cornsilk;
  }
  .history_header_text{
    float: left;
    background-color: inherit;
    padding-left: .5em;
    width: 90%;
  }
  .hits{
    position: relative;
    z-index: 10;
  }
  ul{
    width: 100%;
    position: absolute;
    list-style-type: none;
    margin-left: -3em;
    padding: 0;
    margin: 0;
    background-color: inherit;
  }
  li{
    width: 100%;
    padding: 0;
    margin: 0;
  }
  li div{
    padding-left: 1em;
  }
  .history_query{
    float: left;
    width: 60%;
    background-color: inherit;
  }
  .history_timestamp{
    float: left;
    width: 30%;
    font-size: .7em;
    color: darkgray;
    text-align: right;
    padding: .5em 1em .2em 0;
    background-color: inherit;
  }
  .button{
    display: inline;
    background-color: inherit;
    color: darkgray;
    cursor: pointer;
  }
  .button:hover{
    color:cadetblue;
  }
</style>
