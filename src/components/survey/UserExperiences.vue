<template>
  <section>
    <base-card>
      <h2>Submitted Experiences</h2>
      <div>
        <base-button @click="loadExperiences">Load Submitted Experiences</base-button>
      </div>
      <p v-if="isLoading">Loading...</p>
      <p v-else-if="!isLoading && errMessage"> {{ errMessage }}</p>
      <p v-else-if="!isLoading && (!results || results.length === 0)">No stored experiences.</p>
      <ul v-else>
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
    return {
      results: [],
      isLoading: false,
      errMessage: null
    }
  },
  methods: {
    async loadExperiences(){
      //fetching our data from firebase db
      // fetch('https://vue-http-demo-bb4da-default-rtdb.firebaseio.com/surveys.json', {
      //   // the deafult method is GET, therefore we don't need to add the methods
      //   method: 'GET',
      //   // since we don't have a body, we also don't need the headers
      //   headers: {
      //     'Content-Type': 'application/json'
      //   }
      // });
      this.isLoading = true;
      try {
        const response = await fetch('https://vue-http-demo-bb4da-default-rtdb.firebaseio.com/surveys.json');
        const jsonResponse = await response.json();
        this.isLoading = false;
        this.errMessage = null;
        const results = [];
        for (const id in jsonResponse){
          results.push({
            id: id,
            name: jsonResponse[id].name,
            rating: jsonResponse[id].rating
          })
          this.results = results;
        }  
      } catch (error) {
        this.isLoading = false;
        this.errMessage = 'Failed to fetch data - please try again';
      }
      
      
      
    
    }
  },
  mounted(){
    this.loadExperiences();
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