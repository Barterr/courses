<template>
  <div class="col-sm-6 col-md-4">
    <div class="panel panel-success">
      <div class="panel-heading">
        <h3 class="panel-title">{{stock.name}}</h3>
        <small>(Price: {{stock.price}})</small>
      </div>

      <div class="panel-body">
        <div class="pull-left">
          <input type="number"
                 class="form-control"
                 placeholder="Quantity"
                 v-model.number="quantity"
                 :class="{danger: insufficientFunds}">
        </div>
        <div class="pull-right">
          <button class="btn btn-success"
                  :class="{'btn-danger': insufficientFunds, 'btn-success': !insufficientFunds}"
                  @click="buyStock"
                  :disabled="quantity <= 0 || !Number.isInteger(this.quantity) || insufficientFunds">
            {{insufficientFunds ? 'No Funds' : 'Buy'}}
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
  .danger {
    border: 1px solid red;
  }
</style>


<script>
  export default {
    props: ['stock'],
    data() {
      return {
        quantity: 0,
      }
    },
    computed: {
      insufficientFunds() {
        return (
          this.quantity * this.stock.price >
          this.$store.getters['portfolio/funds']
        )
      },
    },
    methods: {
      buyStock() {
        const order = {
          stockId: this.stock.id,
          stockPrice: this.stock.price,
          quantity: this.quantity,
        }
        this.$store.dispatch('portfolio/buyStock', order)
        // eslint-disable-next-line
        console.log(order)
        this.quantity = 0
      },
    },
  }
</script>
