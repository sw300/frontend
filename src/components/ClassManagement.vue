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
          title: 'New Class',
          status: 'CREATED'
        },
        course: {}

      }
    },
    created() {

      //var course = {};
      var me = this;

      $.ajax({
        url: "http://localhost:8080/courses/" + this.courseId,
        success:   function(result){
                     me.course = result;

                      $.ajax({
                        url: me.course._links.clazzList.href,
                        success:   function(result){
                          me.clazzes = result._embedded.clazzes;
                        }
                      });

                   },
      })

//       backend.$bind("courses/" + this.courseId, this.course);
//
//       this.course.$load().then(function(course){
//          course.clazzes.$load().then(function(clazzes){
//            me.clazzes = clazzes;
//          })
//       });
    },
    watch: {},
    methods: {
       addClazz(clazz){

         var me = this;

         clazz.course = this.course._links.self.href;

         $.ajax({
           method: "POST",
           url: "http://localhost:8080/clazzes",
           contentType: "application/json",
           data: JSON.stringify(clazz),
           success:
              function(result){
                 me.clazzes.push(result);
                 alert('Successfully Added!')
              },
         })

//         var allClazzes = [];
//         backend.$bind("clazzes", allClazzes);
//         allClazzes.$load();
//
//         allClazzes.$create(clazz).then(function(newClazz){
//          newClazz.$load().course.$set(this.course);
//           me.clazzes.push(newClazz);
//           alert('Successfully Added!')
//         });
       },
       removeClazz(clazz){
          var me = this;

          $.ajax({
            url: clazz._links.self.href,
            method: 'DELETE',
            success:
              function(result){
                var index = me.clazzes.indexOf(clazz);
                me.clazzes.splice(index, 1);
              },
          })
//          clazz.$remove().then(function(){
//            var index = me.clazzes.indexOf(clazz);
//            me.clazzes.splice(index, 1);
//          });
       }
    }
  }
</script>

<style scoped lang="scss" rel="stylesheet/scss">

</style>
