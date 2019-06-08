<template>
  <div class="new-note">
    <label>Title</label>
    <input v-model="note.title" type="text">

    <!-- priority note -->
    <div class="radio-wrapper">
      <div class="priority" v-for="(item, index) in status" :key="index" >
        <input type="radio" v-model="note.status" :id="index"  name="status" :value="item">
        <label :for="index" :class="item"> {{ item }} </label>
      </div>
    </div>

    <label>Description</label>
    <textarea v-model="note.descr"></textarea>
    <button class="btn btnPrimary" @click="addNote">New note</button>
  </div>
</template>

<script>
export default {
  props: {
    note: {
     type: Object,
     required: true
    },
    status: {
      type: Array,
      required: true
    }
  },
  methods: {
    addNote() {
      this.$emit('addNewNote', this.note);
    }
  }
}
</script>

<style lang="scss" scoped>
  .new-note {
    text-align: center;
  }

  textarea {
    margin-bottom: 20px;
  }

  .radio-wrapper {
    display: flex;
    justify-content: space-between;
    width: 40%;
  }

  .priority {
    display: flex;
    align-items: center;
    margin: 10px 0;
    margin-right: 5px;

    input {
      display: block;
      width: auto;
      margin: 0;
      margin-right: 10px;
    }

    label {
      margin: 0;

      &.default {
        color: #0004ff;
      }
      &.warning {
        color: #fed330;
      }
      &.danger {
        color: #fc5c65;
      }
    }
  }
</style>
