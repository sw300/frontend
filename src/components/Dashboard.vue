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

      setInterval(function(){
            $.ajax({
              url: window.backendHost + "/dashboards?sort=totalPrice,desc",
              success:   function(result){
                           me.customers = result._embedded.dashboards;


                         },
            })

      }, 3000)
    },
    watch: {},
    methods: {}
  }
</script>

<style scoped lang="scss" rel="stylesheet/scss">

</style>
