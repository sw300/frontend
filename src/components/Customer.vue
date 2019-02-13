<template>
    <md-card>
      <md-card-header>

        <div class="md-title" v-if="value.customer">{{value.customer.firstName}} {{value.customer.lastName}}</div>
        <md-progress-spinner v-else></md-progress-spinner>

        <div class="md-subhead">
          Total amount: {{value.totalPrice}}
          Total time: {{value.totalTime}}

        </div>

      </md-card-header>
    </md-card>

</template>
<script>
  export default {
    props: {
      value: Object,
    },
    data() {
      return {
      }
    },

   created(){
       this.load();

   },

    watch: {},
    methods: {

      load(){
          var me = this;
          $.ajax({
            url: this.value._links.customer.href,
            success: function(result){
              var value = me.value;
              me.value.customer = result;
              me.value = null;
              me.value = value;
            }
          });
      },

    }
  }
</script>

<style scoped lang="scss" rel="stylesheet/scss">

</style>
