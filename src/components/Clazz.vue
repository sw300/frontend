<template>
    <md-card>
      <md-card-header>
        <div class="md-title">Class</div>
        <div v-if="!editMode" class="md-subhead">
          Status:

          <md-select v-model="value.status">
            <md-option value="open">OPEN</md-option>
            <md-option value="closed">CLOSED</md-option>
            <md-option value="wait-for-instructor">WAIT FOR INSTRUCTOR</md-option>

          </md-select>
        </div>

      </md-card-header>

      <md-card-actions v-if="!editMode">
        <md-button>Enroll</md-button>
        <md-button @click="editMode = true">Edit</md-button>
      </md-card-actions>

      <md-card-actions v-else>
        <md-button v-if="isNew" @click="add">Add</md-button>
        <md-button v-else @click="update">Update</md-button>
      </md-card-actions>

      <md-card-content>

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

      add(){
        this.$emit('input', this.value);
        this.$emit('add', this.value);
      }


    }
  }
</script>

<style scoped lang="scss" rel="stylesheet/scss">

</style>
