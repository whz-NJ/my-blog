<template>
  <div id="single-blog">
    <h1>{{blog.title}}</h1>
    <article>{{blog.body}}</article>
  </div>
</template>

<script>
  export default{
    name:"single-blog",
    data(){
      return{
        id: this.$route.params.id, /*获取到路由参数*/
        blog:{}
      }
    },
    created(){
      this.$http.get('https://jsonplaceholder.typicode.com/posts/' + this.id)
        .then(function(data){
          console.log(data);
          return data.json();
          // this.blog = data.body;
        })
        .then(function(data){
          this.blog = data;
        })
    }
  }
</script>
<style>
  #single-blog{
    max-width: 960px;
    margin: 0 auto;
    padding: 20px;
    background: #eee;
    border: 1px dotted #aaa;
  }
</style>
