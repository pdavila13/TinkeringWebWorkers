<template>
    <div>
        <p>Count numbers: {{ counter }}</p>
        <button onclick="startWorker()">Start Worker</button>
        <button onclick="stopWorker()">Stop Worker</button>
    </div>
</template>

<script>
var w

export default {
  name: 'counter',
  data () {
    return {
      counter: 0
    }
  },
  methods: {
    startWorker: function () {
      console.log('start worker')
      if (typeof (Worker) !== 'undefined') {
        if (typeof (w) === 'undefined') {
          w = new Worker('/js/counter.js')
        }
        w.onmessage = function (event) {
          this.counter = event.data
        }
      } else {
        document.getElementById('result').innerHTML = 'Sorry! No Web Worker support.'
      }
    }
  },
  stopWorker: function () {
    console.log('stop worker')
    w.terminate()
    w = undefined
  }
}
</script>
