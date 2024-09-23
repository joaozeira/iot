<template>
  <div id="app">
    <PatientForm />
  </div>
</template>

<script>
import PatientForm from './components/PatientForm.vue';

export default {
  components: {
    PatientForm
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
  margin-top: 60px;
}
</style>
