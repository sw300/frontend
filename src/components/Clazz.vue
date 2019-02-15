<template>
    <md-card :class="isNew ? 'md-primary' : ''">
      <md-card-header>
        <div class="md-title" v-if="editMode">
          <md-input-container><md-input v-model="value.title"></md-input></md-input-container>
        </div>
        <div class="md-title" v-else>{{value.title}}</div>
        <div class="md-subhead">
          Status:

          <md-select v-if="editMode" v-model="value.status">
            <md-option value="OPEN">OPEN</md-option>
            <md-option value="FINISH">FINISH</md-option>
            <md-option value="CREATED">CREATED</md-option>
          </md-select>
          <div v-else>{{value.status}}</div>
        </div>

      </md-card-header>


      <md-card-actions v-if="!editMode">
        <md-button @click="editMode = true">Edit</md-button>
      </md-card-actions>

      <md-card-actions v-else>
        <md-button v-if="isNew" @click="add">Add</md-button>
        <div v-else>
          <md-input-container>
            <md-input v-model="date" type="date"></md-input>
          </md-input-container>
          <md-button @click="addClassDay">add class day</md-button>
          <md-button @click="update">Update</md-button>
          <md-button @click="remove">Remove</md-button>
        </div>
      </md-card-actions>

      <md-card-content>
        <md-chip v-for="item in value.clazzDays" md-deletable @delete="deleteClassDay(item)">{{formatDate(item.date)}}</md-chip>
      </md-card-content>
    </md-card>

</template>
<script>
  export default {
    props: {
      value: Object,
      editMode: false,
      isNew: false
    },
    created(){
        this.load();

    },
    data() {
      return {
        date: null
      }
    },
    watch: {},
    methods: {
      load(){
          var me = this;
          $.ajax({
            url: this.value._links.clazzDayList.href,
            success: function(result){
              var value = me.value;
              me.value.clazzDays = result._embedded.clazzDayList;
              me.value = null;
              me.value = value;
            }
          });
      },
      update(){

        this.$emit('input', this.value);
        this.$emit('change', this.value);
      },

      add(){
        this.$emit('input', this.value);
        this.$emit('add', this.value);
      },

      remove(){
        this.$emit('input', this.value);
        this.$emit('remove', this.value);
      },

      formatDate(date){

        if(date){
          return date;

        }
//          return date.toLocaleDateString('en-GB', {
//              day : 'numeric',
//              month : 'short',
//              year : 'numeric'
//          }).split(' ').join('-');

        else
          return "";
      },

      addClassDay(){

         var me = this;

         var clazzDay = {
          date: this.date,
          clazz: this.value._links.self.href
         };

        $.ajax({
          method: "POST",
          url: window.backendHost + "/clazzDays",
          contentType: "application/json",
          data: JSON.stringify(clazzDay),
          success:
             function(result){
                me.load();
             },
        })


//         var theClassDays = [];
//         backend.$bind("clazzDays", theClassDays);
//
//         var theClassDay = {
//            date: this.date,
//            clazz: this.value
//         }
//
//         theClassDays.$create(clazz).then(function(newClazz){
//           me.clazzes.push(newClazz);
//           alert('Successfully Added!')
//         });


      },
      deleteClassDay(clazzDay){

          var me = this;

         $.ajax({
           method: "DELETE",
           url: clazzDay._links.self.href,
           success:
              function(result){
                 me.load();
              },
         })

     }



    }
  }
</script>

<style scoped lang="scss" rel="stylesheet/scss">

</style>
