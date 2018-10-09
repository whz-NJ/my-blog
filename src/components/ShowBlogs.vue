<template>
  <div v-theme:column="'wide'" id="show-blogs">
    <h1>博客总览</h1>
    <input type="text" v-model="search" placeholder="搜索">
    <!--vue 不推荐在 v-for 里使用过滤器，推荐使用计算属性-->
    <div v-for="blog in filteredBlogs" class="single-blog">
      <router-link v-bind:to="'/blog/' + blog.id"> <!--to属性只能是字符串，如果要取变量，必须用v-bind-->
        <h2 v-rainbow>{{blog.title | to-uppercase }}</h2>
      </router-link>
      <article>
        {{blog.body | snippet}}
      </article>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'show-blogs',
    data() {
      return {
        blogs:[],
        search: ""
      }
    },
    created() {
      // this.$http.get("https://jsonplaceholder.typicode.com/posts").then(function (data) {
      // this.$http.get("../assets/posts.json").then(function (data) {
      this.$http.get("./../static/posts.json").then(function (data) {
        this.blogs = data.body.slice(0,10);
        console.log(this.blogs);
      }
    ) //vue-resource中的方法
    },
    computed:{
      filteredBlogs:function(){
        return this.blogs.filter((blog) =>{
          return blog.title.match(this.search);
        })
      }
    },
    //局部过滤器（原来在main.js里定义的是全局的）
    filters: {
      // "to-uppercase": function (value) {
      //   return value.toUpperCase();
      // },
      // ES6里新增的语法：
      toUppercase(value){
        return value.toUpperCase();
      }
    },
    //局部指令API
    directives:{
      'rainbow':{
        bind(el,binding,vnode){
          el.style.color = "#" + Math.random().toString(16).slice(2,8);
        }
      }
    }

  }
</script>

<style>
  #show-blogs{
    max-width: 800px;
    margin: 0 auto;
  }
  .single-blog{
    max-width: 960px;
    margin: 0 auto;
    padding: 20px;
    background: #eee;
    border: 1px dotted #aaa;
  }
  #show-blogs a{
    color: #444;
    text-decoration: none;
  }

  input[type="text"]{
    padding: 8px;
    width: 100%;
    box-sizing: border-box;
  }

</style>
