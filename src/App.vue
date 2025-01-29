



<template>
  <div>
    <upload_csv v-on:data="dataframe=$event"/>
  </div>
</template>



<script>
import Upload_csv from './components/upload_csv.vue';
import upload_csv from './components/upload_csv.vue';
export default {

  data() {
    return {
      dataframe:null
    };
  },

  methods: {
  },
  components: {
    upload_csv
    
  }
}
</script>
