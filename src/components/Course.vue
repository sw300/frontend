<template>
    <md-card :class="editMode ? 'md-accent' : ''">
     <!--md-card-media>
        <img src="https://images.careers360.mobi/sites/default/files/field/image/2015/04/08/Courses-1.jpg" alt="People">
      </md-card-media-->

      <md-card-header>
        <div v-if="!editMode" class="md-title">{{value.title}}</div>
        <div v-else class="md-title">
          <md-input-container>
            <label>Course Title</label>
            <md-input type="text"
                      v-model="value.title"></md-input>
          </md-input-container>
        </div>

        <div v-if="!editMode" class="md-subhead">
          Enrollment:
            min: {{value.minEnrollment}},
            max: {{value.maxEnrollment}}
        </div>
        <div v-else class="md-subhead">
          Enrollment:
            <md-input-container>
              <label>min</label>
              <md-input type="number"
                        v-model="value.minEnrollment"></md-input>
            </md-input-container>
            <md-input-container>
              <label>max</label>
              <md-input type="number"
                        v-model="value.maxEnrollment"></md-input>
            </md-input-container>
        </div>
      </md-card-header>

      <md-card-actions v-if="!editMode">
        <md-button>Enroll</md-button>
        <md-button @click="editMode = true">Edit</md-button>
        <md-button @click="remove">Remove</md-button>
        <md-button @click="jumpToClasses">Classes</md-button>
      </md-card-actions>
      <md-card-actions v-else>
        <md-button v-if="isNew" @click="add">Add</md-button>
        <md-button v-else @click="update">Update</md-button>
      </md-card-actions>

      <md-card-content v-if="!editMode">
        {{value.description}}

        <br>

      </md-card-content>
      <md-card-content v-else>

        <md-input-container>
          <label>description</label>
          <md-textarea v-model="value.description"></md-textarea>
        </md-input-container>

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
    data() {
      return {
      }
    },
    watch: {},
    methods: {
      update(){

        this.$emit('input', this.value);
        this.$emit('change', this.value);
      },
      remove(){

        this.$emit('input', this.value);
        this.$emit('remove', this.value);
      },

      add(){
         this.$emit('input', this.value);
         this.$emit('add', this.value);
      },

      jumpToClasses(){
        this.$emit('classes', this.value);
      }


    }
  }
</script>

<style scoped lang="scss" rel="stylesheet/scss">

</style>
