<template>
  <section>
    <base-card>
      <h2>Submitted Experiences</h2>
      <div>
        <base-button @click="loadSurvey">Load Submitted Experiences</base-button>
      </div>
      <p v-if="isLoading">Loading...</p>
      <p v-else-if="error">{{ error }}</p>
       <p v-else-if="!isLoading && (!results || results.length === 0)">There's no data. Start adding one!</p>
      <ul v-else-if="!isLoading && results && results.length > 0">
        <survey-result
          v-for="result in results"
          :key="result.id"
          :name="result.name"
          :rating="result.rating"
        ></survey-result>
      </ul>
    </base-card>
  </section>
</template>

<script>
import SurveyResult from './SurveyResult.vue';

export default {
 
  components: {
    SurveyResult,
  },
  data(){
    return{
      results:[],
      isLoading: false,
      error: null
    }
  },  
  methods:{
    async loadSurvey(){
      this.isLoading = true;
      this.error = null;
      let result;
      let jsonRes
      try{
         result = await fetch('https://vue-http-requests-f0f4d-default-rtdb.asia-southeast1.firebasedatabase.app/survey.json');
         jsonRes = await result.json();
         this.isLoading = false;
      }catch(error){
        this.isLoading = false;
        console.log(error);
        this.error = 'Error in Fetching Data. Please try again later.'
      }
      const results = [];
      for(const id in jsonRes){
        results.push({
          id: id,
          name: jsonRes[id].name,
          rating: jsonRes[id].rating
        })
      }
      this.results = results;
    }
  },
  mounted(){
    this.loadSurvey();
  }
};
</script>

<style scoped>
ul {
  list-style: none;
  margin: 0;
  padding: 0;
}
</style>