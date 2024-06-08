<template>
  <div id="app">
    <h1 class="text-4xl text-white font-bold">Beaver Notes API Playground</h1>
    <request-auth></request-auth>
    <confirm-auth></confirm-auth>
    <add-note></add-note>
    <delete-note></delete-note>
    <add-label></add-label>
  </div>
</template>

<script>
import RequestAuth from './components/RequestAuth.vue';
import ConfirmAuth from './components/ConfirmAuth.vue';
import AddNote from './components/AddNote.vue';
import DeleteNote from './components/DeleteNote.vue';
import AddLabel from './components/AddLabel.vue';

export default {
  components: {
    RequestAuth,
    ConfirmAuth,
    AddNote,
    DeleteNote,
    AddLabel,
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
