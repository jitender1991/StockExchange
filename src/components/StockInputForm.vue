<template>
    <div class="col-9">
      <form class="d-inline" @submit.prevent="submitStockId()">
        <input  
          v-model="newStockID"
          type="text"
          class="form-control col-10 d-inline"
          @focus="showStockListMenu($event)"
          placeholder="Search for a new Stock..."
        />
      </form>
      <button class="d-inline btn btn-secondary dropdown-toggle" type="button" id="dropdownMenuButton" @click="showStockListMenu($event)">
      </button>
      <div v-if="showStockList ">
          <li class="d-flex col-10 align-items-center list-group-item" v-for="PreDefineStock in PreDefinedStockList" v-bind:key="PreDefineStock" @click="submitStockId(PreDefineStock)">
            {{ PreDefineStock }}
          </li>
        </div>
    </div>
</template>

<script>

export default {
  data() {
    return {
      newStockID: "",
      showStockList : false
    };
  },

  props: {
    PreDefinedStockList: Array
  },
  
  methods: {
    submitStockId(stockId) {
      if(stockId){
        this.newStockID = stockId;
        this.showStockList = false;
      }
      if(this.newStockID.length > 0) {
        this.$emit("on-new-stockData", this.newStockID);
      }
      this.newStockID = "";
    },
    showStockListMenu(event){
      if(event.target.type === "text"){
        this.showStockList = false;
      }
      else if(!this.showStockList){
        this.showStockList = true
      }
      else {
        this.showStockList= false
      }
    }
  }
};
</script>

<style lang="scss" scoped></style>
