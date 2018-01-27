<template>
  <div>
    <h1>Class Management</h1>

    <clazz v-model="clazzes[index]" v-for='(clazz, index) in clazzes' @remove="removeClazz"></clazz>
    <clazz v-model="newClazz" editMode=true isNew=true @add="addClazz"></clazz>

  </div>
</template>
<script>
  export default {
    props: {courseId: String},
    data() {
      return {
        clazzes: [],
        newClazz: {
          title: 'New Class'
        }

      }
    },
    created() {

      var course = {};
      var me = this;

       backend.$bind("courses/" + this.courseId, course);

       course.$load().then(function(course){
          course.clazzes.$load().then(function(clazzes){
            me.clazzes = clazzes;
          })
       });
    },
    watch: {},
    methods: {
       addClazz(clazz){

         var me = this;
         var allClazzes = [];
         backend.$bind("clazzes", allClazzes);
         allClazzes.$load();

         allClazzes.$create(clazz).then(function(newClazz){
           me.clazzes.push(newClazz);
           alert('Successfully Added!')
         });
       },
       removeCourse(clazz){
          var me = this;
          clazz.$remove().then(function(){
            var index = me.clazzes.indexOf(clazz);
            me.clazzes.splice(index, 1);
          });
       }
    }
  }
</script>

<style scoped lang="scss" rel="stylesheet/scss">

</style>
