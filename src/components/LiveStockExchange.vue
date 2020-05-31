<template>
  <div class="container">
    <div class="row">
      <div class="col-12 py-5">
        <h1>Live Stock Exchange</h1>
        <h3>{{ networkStatus }}</h3>
      </div>
    </div>
    <div class="row mb-3">
      <stock-input-form
        @on-new-stockData="addNewStockData($event)"
        v-bind:PreDefinedStockList="preDefinedStockList"
      />
    </div>
    <h5 v-if="stockAlreadyInList">Stock Already In List</h5>
    <div class="row">
      <div class="col-lg-8">
        <ul class="list-group">
          <stock-list-view
            v-for="stock in stocksData"
            v-bind:key="stock.isin"
            :isin="stock.isin"
            :price="stock.price"
            :bid="stock.bid"
            :ask="stock.ask"
            @on-delete="deleteStock(stock)"
          />
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
import StockListView from "./StockListView.vue";
import StockInputForm from "./StockInputForm.vue";
import webSocket from "../WebSocket.js";
import preDefinedStockList from "../preDefinedStockList.js";
import AppConstants from "../AppConstants";

export default {
  data() {
    return {
      stocksData: {},
      stockAlreadyInList : false,
      preDefinedStockList: preDefinedStockList,
      networkStatus: AppConstants.waitingForConnection
    };
  },

  created(){
    var that = this;

    webSocket.onopen = function(){
      that.networkStatus = AppConstants.onlineMessage;
    };

    webSocket.onmessage = function(event) {
      var newStockData = JSON.parse(event.data);
      if(that.deletedStockId && newStockData.isin === that.deletedStockId){
        clearTimeout(timer);
        var timer = setTimeout(function(){
          that.deletedStockId =  null;
        }.bind(that),1000);
      }
      else {
        that.$set(that.stocksData, newStockData.isin , newStockData);
      }
    };

    webSocket.onerror = function(error) {
      that.networkStatus = AppConstants.waitingForConnection;
    };

    navigator.connection.onchange = function(event){
      that.networkStatus = navigator.onLine ? AppConstants.onlineMessage : AppConstants.offlineMessage; 
    };
    
  },

  methods: {
    addNewStockData(newStockID) {
      if(this.stocksData.hasOwnProperty(newStockID)){
          this.stockAlreadyInList = true;
          clearTimeout(timer);
          var timer = setTimeout(function(){
            this.stockAlreadyInList = false;
          }.bind(this),1500)
      }
      else {
        webSocket.send(JSON.stringify({"subscribe": newStockID}));
      }
    },
    
    deleteStock(stock){
      webSocket.send(JSON.stringify({"unsubscribe": stock.isin}));
      this.deletedStockId = stock.isin;
      this.$delete(this.stocksData , stock.isin);
    }

  },
  components: { StockListView, StockInputForm },
};
</script>

<style scoped lang="scss"></style>
