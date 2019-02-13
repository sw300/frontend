<template>
  <div>
    <h1> Top Customers </h1>

    <customer v-model="customers[index]" v-for="(customer, index) in customers"></customer>

  </div>
</template>
<script>
  export default {
    props: {},
    data() {
      return {
        customers: []
      }
    },
    created() {

      //var course = {};
      var me = this;

      $.ajax({
        url: "http://localhost:8080/dashboards?sort=totalPrice,desc",
        success:   function(result){
                     me.customers = result._embedded.dashboards;


                   },
      })
    },
    watch: {},
    methods: {}
  }
</script>

<style scoped lang="scss" rel="stylesheet/scss">

</style>
