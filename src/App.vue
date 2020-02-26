<template>
  <div id="app">
    {{ metrics }}
  </div>
</template>

<script>
  import axios from 'axios';

  export default {
    name: 'app',
    data(){
      return {
        metrics: ''
      }
    },
    beforeDestroy() {
      this.stopTimer()
    },
    mounted() {
      this.startTimer();
    },
    methods: {
      stopTimer() {
        if (this.interval) {
          clearInterval(this.interval)
        }
      },
      startTimer() {
        this.stopTimer();
        this.interval = setInterval(() => {
          axios
            .get('http://localhost:9090/api/v1/query?query=go_memstats_alloc_bytes')
            .then(response => (
              this.metrics = response.data.data.result[0].value[1]
            ))
        }, 500)
      },
    }
  }
</script>
