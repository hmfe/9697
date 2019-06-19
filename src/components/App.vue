<template>
  <div id="search_box">
    <SearchBox />
  </div>
</template>  
<script>
  import SearchBox from "./SearchBox"
  export default {
    components: { SearchBox },
  }
</script>
<style>
  #search_box{
    width: 400px;
    max-width: 400px;
    margin: auto;
    font-size: 2vw;
  }
  @media only screen and (max-width: 399px) {
    #search_box{
      width: 300px;
      max-width: 300px;
      margin: auto;
      font-size: 3vw;
    }
  }
  @media only screen and (max-width: 767px) {
   #search_box{
    width: 300px;
    max-width: 300px;
    margin: auto;
    font-size: 3vw;
  }
}
</style>