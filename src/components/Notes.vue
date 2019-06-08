<template>
  <div class="notes">
    <div class="note" :class="[{ full: !grid }, note.status]" v-for="(note, index) in notes" :key="index">
      <div class="note-header" :class="{ full: !grid }">
        <div class="wrapper-edit">
          <p title="Click on edit" :class="note.status" @click="showEdit(index)"> {{ note.title }} </p>
          <input @blur="note.edit = false" @keyup.esc="cancelEdit(index)" type="text" v-model="note.title" v-show="note.edit" @keyup.enter="noteEdit(index)" autofocus>
        </div>
        <p style="cursor: pointer;" @click="removeNote(index)">x</p>
      </div>
      <div class="note-body">
        <p> {{ note.descr }} </p>
        <span> {{ note.date }} </span>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  props: {
    notes: {
      type: Array,
      required: true
    },
    grid: {
      type: Boolean,
      required: true
    }
  },
  data() {
    return {
      writeNote: null
    }
  },
  methods: {
    removeNote(index) {
      console.log(`Note id - ${index} removed`);
      this.$emit('remove', index);
    },
    showEdit(index) {
      this.writeNote = this.notes[index].title;
      this.notes[index].title = '';
      this.notes[index].edit = true;
    },
    cancelEdit(index) {
        this.notes[index].edit = false;
        this.notes[index].title = this.writeNote;
    },
    noteEdit(index) {
      // console.log(this.notes[index].title)
      this.$emit('editNote', index);
    }
  }
}
</script>

<style lang="scss">
  .notes {
    display: flex;
    align-items: center;
    justify-content: space-between;
    flex-wrap: wrap;
    padding: 40px 0;
  }

  .note {
    width: 48%;
    padding: 18px 20px;
    margin-bottom: 20px;
    background-color: #fff;
    transition: all .25s cubic-bezier(.02,.01,.47,1);
    box-shadow: 0 30px 30px rgba(0,0,0,.02);
      &.default {
        border: #0004ff 2px solid;
      }
      &.warning {
        border: 2px solid #fed330;
      }
      &.danger {
        border: 2px solid #fc5c65;
      }

    &:hover {
      box-shadow: 0 30px 30px rgba(0,0,0,0.04);
      transform: translate(0,-6px);
      transition-delay: 0s !important;
    }

    &.full {
      width: 100%;
    }
  }

  .note-header {
    display: flex;
    align-items: center;
    justify-content: space-between;
    margin-top: 20px;

    h1 {
      font-size: 32px;
    }

    p {
      color: #0004ff;
      font-size: 22px;
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

    svg {
      margin-right: 12px;
      color: #999;
      cursor: pointer;

      &.active {
        color: #0004ff;
      }

      &:last-child {
        margin-right: 0;
      }
    }
  }

  .note-body {
    p {
      margin: 20px 0;
    }

    span {
      font-size: 14px;
      color: #999999;
    }
  }

</style>

