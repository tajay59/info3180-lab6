<template>
    <form @submit.prevent="searchNews" class="d-flex flex-column justify-content-center">
        <div class="input-group mx-sm-3 mb-2">
            <label class="visually-hidden" for="search">Search</label>
            <input type="search" name="search" v-model="searchTerm" id="search" class="form-control mb-2 mr-sm-2" placeholder="Enter search term here" />
            <button class="btn btn-primary mb-2">Search</button>
        </div>
        <p>You are searching for {{ searchTerm }}</p>
    </form>

      <ul class="news__list">
          <li v-for="article in articles" class="news__item">
              <div class="card">
                  <img id="cardimg" v-bind:src=article.urlToImage alt="tried">
                  <div class="card_body">                    
                    <div class="headline">{{article.title}}</div>
                    <div class="content"> {{article.description}}</div>
                    <a id="readmore" v-bind:href=article.url>Read more</a>
                  </div>
                  
                
              </div>   
              </li>
      </ul>

</template>

<script>
console.log( import.meta.env)
export default {
    
    data() {  return { 
        articles:[],
        searchTerm:""
        }; },

    created() { 
        let self = this;
        fetch('https://newsapi.org/v2/top-headlines?country=us',{
            headers: {'Authorization': `Bearer ${import.meta.env.VITE_NEWSAPI_TOKEN}` }})
            .then(function(response) {
                return response.json();
            })
            .then(function(data) {
            console.log(data);
            self.articles = data.articles;
            });
            },

    methods: {
        searchNews() {
            let self = this;
            fetch(`https://newsapi.org/v2/everything?q=${self.searchTerm}&language=en`, {
            headers: { 'Authorization': `Bearer ${import.meta.env.VITE_NEWSAPI_TOKEN}`}})
            .then(function(response) {  return response.json(); })
            .then(function(data) {
                console.log(data);
                self.articles = data.articles;
                });
            }
        }



};
</script>

<style>
/* Add any component specific styles here */
@import url('https://fonts.googleapis.com/css2?family=Ubuntu+Condensed&display=swap');

ul{
    display: grid;
    grid-template-columns: repeat(3,1fr);
    grid-auto-rows: auto;
    grid-gap :30px;
}

li.news__item{
    list-style: none; 
    border: 1px solid lightgray;
    border-radius: 10px;
    border-bottom: 5px solid lightseagreen;
    box-shadow: 0 4px 8px 0 rgba(0,0,0,0.2);
    overflow: hidden; 
}

.card{
     
    display: grid;
    grid-template-columns: 1fr;
    grid-template-rows: 1fr 1fr;
   
}
 #cardimg{
        width: 100%;
        height:300px; 
        object-fit: cover;
       
    }

.card_body{
    width: 100%;
    display: flex;
    flex-direction: column;
    gap:20px;
    padding: 20px;
    
}

.headline{
    word-wrap: break-word;
    font-weight: bold;
    text-align: left;
    font-family: 'Ubuntu Condensed', sans-serif;
    font-size: 20px;
}
.content{
    color: gray;
    word-wrap: break-word;
    text-align: justify;
}

#readmore,#readmore:link{
    width: 100px;
    height:30px; 
    
    border:1px ;
    border-radius: 5px;
    border-bottom: 2px solid lightskyblue;
    text-align: center; 
    vertical-align: middle;
    text-decoration: none;
    font-weight: bold;
    font-size:15px;
}

#readmore:hover{
   background-color: rgba(144, 238, 144, 0.277); 
}

</style>