<template>
  <div>
    <v-container>
      <h1>Job Board</h1>
      <div v-for="job in jobs" :key="job.pk">
        <h2>
          <router-link
           :to="{ name: 'job', params: { id: job.id }}"
           class="job-link"
          >{{ job.company_name }}
          </router-link>
        </h2>
        <p>{{ job.job_title }}</p>
        <hr>
      </div>
      <div class="my-4">
        <p v-show="loadingJobs">...loading...</p>
        <v-btn
            id="success"
            v-show="next"
            @click="getJobs"
            color="success"
          >Load More
        </v-btn>
      </div>
    </v-container>
  </div>
</template>

<script>
import { apiService } from '../common/api.service.js'

export default {
  name: "home",
  data(){
    return{
      jobs: [],
      next: null,
      loadingJobs: false
    }
  },
  methods: {
    getJobs(){
      let endPoint = `api/jobs/`
      if(this.next){
        endPoint = this.next;
      }
      this.loadingJobs = true;
      apiService(endPoint).then(data => {
        this.jobs.push(...data.results)
        this.loadingJobs = false;
        if(data.next){
           this.next = data.next;
        }else{
           this.next = null;
        }
      })
    }
  },
  created(){
    this.getJobs()
    document.title = 'Job Board';
  }
};
</script>

<style scoped>
 .job-link{
   font-weight: bold;
   color:black;
   text-decoration: none;
 }
 .job-link:hover{
   color:#418883;
 }
</style>
