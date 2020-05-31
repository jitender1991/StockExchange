<template>
  <li class="d-flex align-items-center list-group-item">
        <h5 class="title d-inline col-3">ISIN: {{ isin }}</h5>
        <div class="d-inline col-3"> Price :{{ price.toFixed(1) }}</div>
        <div class="d-inline col-3">Bid :{{ bid.toFixed(1) }}</div>
        <div class="d-inline col-2">Ask :{{ ask.toFixed(1) }}</div>
        <button class="btn" @click="$emit('on-delete')">
          <span class="fa fa-trash"></span>
        </button>
  </li>
</template>

<script>
export default {
  props: {
    isin: [String, Number],
    price: Number,
    bid: Number,
    ask: Number
  }
};
</script>

