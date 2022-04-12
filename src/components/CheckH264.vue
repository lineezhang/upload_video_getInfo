<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <div>
        <input @change="onChange($event)" type="file" style="background-color: aquamarine;" id="file" name="file"></input>
    </div>
  </div>
</template>

<script>
import mediainfo  from 'mediaInfo.js'
export default {
  name: 'CheckH264',
  props: {
    msg: String
  },
  methods:{
      onChange(e){
        console.log(e.target.files[0])
        const file = e.target.files[0]
        if(!file) return
          
        const getSize = () => file.size
        const readChunk = (chunkSize, offset) =>
            new Promise((resolve, reject) => {
                const reader = new FileReader()
                reader.onload = (event) => {
                    if (event.target.error) {
                        reject(event.target.error)}
                    resolve(new Uint8Array(event.target.result))
                }
                reader.readAsArrayBuffer(file.slice(offset, offset + chunkSize))
           })
                 
            
                  mediainfo().then((mediainfo) =>{
                    mediainfo
                    .analyzeData(getSize, readChunk)
                    .then((result) => {
                        console.log("result",result)
                      if (result['media'] !== undefined) {
                        console.log('Format',result.media.track[1].Format)
                        let track = result['media']['track']
                        if (track !== undefined) {
                            console.log(result.media.track[1].Format)
                            	if (result.media.track[1].Format != "AVC") {
                                 console.log('不是h264视频格式')
                            }
                        }
                      }
                    })
                    .catch((error) => {
                      console.log(`An error occured:\n${error.stack}`)
                    })
                  })
         
      }
      
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
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
