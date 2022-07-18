<template>
  <img alt="Vue logo" src="./assets/logo.png">

  <br>

  <button @click="populateQueue()">Populate Queue</button>

  <br>

  <button @click="ProcessQueue()">Process Queue</button>
  <br>

  <button @click="clearPendingJobs()">Clear Pending Jobs</button>

  <br>

  <button @click="clearCompletedJobs()">Clear Completed Jobs</button>

  
    <div>
        <h2>Active</h2>
        <div>{{ active.id }}</div>

        <div style="display: flex; margin-top: 20px; align-items: start; justify-content: space-between; width: 75%; margin: auto">
            <div>
                <h2>Pending</h2>
                <div v-for="( job, index ) in pending"
                    v-bind:key="'pending-'+index">
                        {{ job }}

                        <button v-on:click="cancelJob( index )">Cancel Job</button>
                </div>
            </div>
            <div>
                <h2>Completed</h2>
                <div v-for="( job, index ) in completed"
                    v-bind:key="'completed-'+index">
                        {{ job }}
                </div>
            </div>
        </div>
        
    </div>

</template>

<script>

// import QueueController from './components/QueueController.vue';
import { mapState } from 'vuex';
export default {
  name: 'App',
  // components: {
  //   QueueController
  // },
  data() {
    return {
      id: 0,
    }
  },
  computed: {
        ...mapState('queue', {
            pending: 'pending',
            active: 'active',
            completed: 'completed'
        })
    },

  watch: {
        active(){
            this.processJob();
        }
    },
  methods: {
    populateQueue(){
        this.id++
        this.$store.dispatch('queue/addJob', {
          id: this.id
        });
    },

    ProcessQueue() {
      this.$store.dispatch('queue/startNextJob');
    },  

    clearPendingJobs(){
      this.$store.dispatch('queue/clearPendingJobs');
    },

    clearCompletedJobs(){
      this.$store.dispatch('queue/clearCompletedJobs');
    },

    processJob(){
            if( this.active.handler ){
                this.active.handler();
            }
        },

        cancelJob( index ){
            this.$store.dispatch('queue/cancelJob', index );
        }
  }
}
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
