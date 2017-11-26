<template>
  <div class="hello">
    <img ref="img" :src="src" :alt="err">
    <canvas ref="canvas"></canvas>
  </div>
</template>

<script>
/* global cv */
import axios from 'axios'
export default {
  name: 'HelloWorld',
  data () {
    return {
      src: null,
      err: null,
      handler: null
    }
  },
  created () {
    // TODO: hide in logic
    axios.put('http://orange.local:4035/gotapi/mediastream_recording/preview?serviceId=Host.b89e626b46627ebc83218cdabaa1e3d2')
    .then((res) => {
      this.src = res.data.uri.replace('localhost', 'orange.local')
      this.handler = setInterval(() => {
        let img = this.$refs.img
        let canvas = this.$refs.canvas
        let ctx = canvas.getContext('2d')
        ctx.drawImage(img, 0, 0)

        let src = cv.imread(canvas)
        console.debug(src)
      }, 2000)
    })
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>
