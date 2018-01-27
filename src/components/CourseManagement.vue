<template>
  <div>
    <h1>Course Management</h1>

    <course v-model="courses[index]" v-for='(course, index) in courses' @change="updateCourse" @remove="removeCourse" @classes="jumpToClasses"></course>

    <h2>Add New Course</h2>
    <course v-model="newCourse" editMode=true isNew=true @add="addCourse"></course>

  </div>
</template>
<script>
  export default {
    props: {},
    data() {
      return {
        courses: [],
        newCourse: {
          title: 'New Course'
        }

      }
    },
    created() {

      var courses = [];
      var me = this;

       backend.$bind("courses", courses);

       courses.$load().then(function(courses){
          me.courses = courses; // set the view's data with the loaded data obtained from backend.
       });
    },
    watch: {},
    methods: {
      updateCourse(course){
        course.$save().then(function(){
          alert('Successfully Updated!')
        });
      },
       addCourse(course){

         var me = this;
         this.courses.$create(course).then(function(newCourse){
           me.courses.push(newCourse);
           me.newCourse = {title: 'New Course'};

           alert('Successfully Added!');

         });


       },
       removeCourse(course){
          var me = this;
          course.$remove().then(function(){
            var index = me.courses.indexOf(course);
            me.courses.splice(index, 1);
          });
       },
       jumpToClasses(course){
          window.location="/classes/1";

       }
    }
  }
</script>

<style scoped lang="scss" rel="stylesheet/scss">

</style>
