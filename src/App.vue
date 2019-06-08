<template>
<div class="wrapper">

 <div class="wrapper-content">
   <section>
    <div class="container">

      <!-- message -->
      <message v-if="message" :message="message"/>

      <!-- new note -->
      <newNote
      :note="note"
      :status="status"
      @addNewNote="addNote"/>

      <!-- title -->
      <div class="note-header">
        <h1> {{title}} </h1>

        <!-- search -->
        <search
          :value="search"
          placeholder="Find your note"
          @search="search = $event"/>

        <!-- icons controls -->
        <div class="icons">
          <svg :class="{ active: grid }" @click="grid = true" xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" ><rect x="3" y="3" width="7" height="7"></rect><rect x="14" y="3" width="7" height="7"></rect><rect x="14" y="14" width="7" height="7"></rect><rect x="3" y="14" width="7" height="7"></rect></svg>

          <svg :class="{ active: !grid }" @click="grid = false" xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><line x1="8" y1="6" x2="21" y2="6"></line><line x1="8" y1="12" x2="21" y2="12"></line><line x1="8" y1="18" x2="21" y2="18"></line><line x1="3" y1="6" x2="3" y2="6"></line><line x1="3" y1="12" x2="3" y2="12"></line><line x1="3" y1="18" x2="3" y2="18"></line></svg>
        </div>
      </div>

      <!-- notes list -->
      <notes @editNote="editNote" :notes="notesFilter" :grid="grid" @remove="removeNote"/>

    </div>
   </section>

  </div>
</div>
</template>

<script>
import message from '@/components/Message.vue';
import newNote from '@/components/NewNote.vue';
import notes from '@/components/Notes.vue';
import search from '@/components/Search.vue';

export default {
  components: {
    message,
    newNote,
    notes,
    search
  },
  data() {
    return {
      title: 'Note App',
      search: '',
      message: null,
      grid: true,
      status: [
        'default',
        'warning',
        'danger'
      ],
      note: {
        title: '',
        descr: '',
        status: 'default',
        edit: false
      },
      notes: [
        {
          title: 'First title',
          descr: 'Description for first note',
          status: 'default',
          edit: false,
          date: new Date(Date.now()).toLocaleString()
        },
        {
          title: 'Second title',
          descr: 'Description for second note',
          status: 'warning',
          edit: false,
          date: new Date(Date.now()).toLocaleString()
        },
        {
          title: 'Third title',
          descr: 'Description for third note',
          status: 'danger',
          edit: false,
          date: new Date(Date.now()).toLocaleString()
        },
      ]
    }
  },
  computed: {
    notesFilter() {
      let array = this.notes,
          search = this.search;
      if (!search) {
        return array;
      }
      // small
      search = search.trim().toLowerCase();
      //filter
      array = array.filter(function(item) {
        if (item.title.toLowerCase().indexOf(search) !== -1) {
          return item;
        }
      });
      //error
      return array;
    }
  },
  methods: {
        addNote() {
          let {title, descr, status} = this.note;
          let date = new Date(Date.now()).toLocaleString();

          if (title === '') {
            this.message = 'title cant\'t be blank';
            return false;
          }

          this.notes.push({
            title,
            descr,
            status,
            date
          });

        console.log(status);
          this.note.title = '';
          this.note.descr = '';
          this.note.status = 'default';
          this.message = null;
        },
        removeNote(index) {
          this.notes.splice(index, 1);
        },
        editNote(index) {

          this.notes[index].edit = false;
          // console.log(text)
        }
      }
}
</script>

<style>

</style>
