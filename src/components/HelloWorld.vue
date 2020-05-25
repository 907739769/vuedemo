<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <button v-on:click="loadMore">click</button>
    <ul v-for="(imageList,index) in imageLists" :key="index">
      <li v-for="(item,id) in imageList" :key="id">
        <a v-bind:href="item.url">
          <img alt="baby" class="simg" mode="aspectFit" v-bind:src="item.thumbUrl"/>
        </a>
      </li>
    </ul>
  </div>
</template>

<script>
  import axios from 'axios'

  export default {
    name: 'HelloWorld',
    props: {
      msg: String,
      // url: String
    },
    data() {
      return {
        // title: "静态数据",
        // url: "e",
        // thumbUrl:"e"
        imageList:[],
        imageLists:[],
        imgListIndex:0,
        pageNum:1,
        pageSize:24,
      }
    },
    //在这里调用ajax请求方法
    created(){
      this.convert();
    },
    methods: {
      convert: function () {
        axios.get("http://127.0.0.1:8101/image/get",{
          params:{
            "pageNum":this.pageNum,
            "pageSize":this.pageSize,
          }
        }).then(res => {
          // this.thumbUrl = res.data.records[0].thumbUrl;
          // this.url = res.data.records[0].url;
          this.imageLists.push(res.data.records);
          //this.imageLists[this.imgListIndex]=res.data.records;
          //console.log(this.imageLists);
        })
        this.pageNum++;
      },
      loadMore: function () {
        axios.get("http://127.0.0.1:8101/image/get",{
          params:{
            "pageNum":this.pageNum,
            "pageSize":this.pageSize,
          }
        }).then(res => {
          // this.thumbUrl = res.data.records[0].thumbUrl;
          // this.url = res.data.records[0].url;
          this.imageLists.push(res.data.records);
          console.log(this.imageList);
          //this.imageLists[this.imgListIndex]=res.data.records;
          //console.log(this.imageLists);
        })
        this.pageNum++;
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
    margin: 0 0;
  }
  a {
    color: #42b983;
    margin: 0.3125rem;
  }
  .simg{
    width: auto;
    /* height: 20rem */
  }
</style>
