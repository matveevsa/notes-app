<template>
  <div class="edit-wrapper">
    <div class="edit-content">
      <input type="text" v-model="notes.t">
    </div>
  </div>
</template>

<script>
export default {
  props: {
    notes: {
      type: Array,
      required: true
    }
  }
}
</script>
