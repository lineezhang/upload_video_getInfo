<template>
    <div>
        <h1>{{ msg }}</h1>
        <div>
            <input type="file" @change="onChange($event)" style="background-color: aquamarine;" id="file" name="file"></input>
            <button id="submit">上传</button>
        </div>
    </div>
</template>

<script>
    import OSS from "ali-oss";
export default {
    name: 'OssUpload',
    props: {
        msg: String
    },
    data() {
        return {
            client:null,
            
        }
    },
    methods: {
        onChange(evt){
            const file = evt.target.files[0]
            
            console.log(file)
            
            this.putObject(file)
            
          
        },
        
        async putObject (data) {
        const options = {
        // 获取分片上传进度、断点和返回值。
        progress: (p, cpt, res) => {
          console.log(p);
        },
        // 设置并发上传的分片数量。
        parallel: 4,
        // 设置分片大小。默认值为1 MB，最小值为100 KB。
        partSize: 1024 * 1024,
        // headers,
        // 自定义元数据，通过HeadObject接口可以获取Object的元数据。
        meta: { year: 2020, people: "test" },
        mime: "text/plain",
      };
     try {
           // 填写Object完整路径。Object完整路径中不能包含Bucket名称。
           // 您可以通过自定义文件名（例如exampleobject.txt）或文件完整路径（例如exampledir/exampleobject.txt）的形式实现将数据上传到当前Bucket或Bucket中的指定目录。
           // data对象可以自定义为file对象、Blob数据或者OSS Buffer。
          const result = await this.client.multipartUpload(
            "test/b.jpg",
            data,
            //{headers}
            options
          );
          console.log(result);
        } catch (e) {
          console.log(e);
        }
      }
        
        
    },
    created() {
         this.client = new OSS({
                // yourRegion填写Bucket所在地域。以华东1（杭州）为例，Region填写为oss-cn-hangzhou。
        region: "",
        // 从STS服务获取的临时访问密钥（AccessKey ID和AccessKey Secret）。
        accessKeyId: "",
        accessKeySecret: "",
        // 从STS服务获取的安全令牌（SecurityToken）。
        stsToken: ",
        // 填写Bucket名称，例如examplebucket。
        bucket: "",
      });

    }
}
</script>

<style scoped>
</style>
