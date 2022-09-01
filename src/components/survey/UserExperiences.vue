<template>
  <section>
    <base-card>
      <h2>Submitted Experiences</h2>
      <div>
        <base-button @click="loadSurvey">Load Submitted Experiences</base-button>
      </div>
      <ul>
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
      results:[]
    }
  },  
  methods:{
    async loadSurvey(){
      const result = await fetch('https://vue-http-requests-f0f4d-default-rtdb.asia-southeast1.firebasedatabase.app/survey.json');
      const jsonRes = await result.json();
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