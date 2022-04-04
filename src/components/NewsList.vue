<template>
      <ul class="news__list">
          <li v-for="article in articles" class="news__item">
              <div class="card">
                  <img id="cardimg" v-bind:src=article.urlToImage alt="tried">
                  <div class="card_body">                    
                    <div class="headline">{{article.title}}</div>
                    <div class="content"> {{article.content}}</div>
                  </div>
                
              </div>
              
              
              </li>
      </ul>
</template>

<script>
console.log( import.meta.env)
export default {
    
    data() {  return { articles:[]}; },

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

li{
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
    grid-template-rows: 1fr 2fr;
   
}
 #cardimg{
        width: 100%;
        height: 200px;
        object-fit: cover;
       
    }

.card_body{
    width: 100%;
    display: grid;
    grid-template-columns: 1fr;
    grid-template-rows: 1fr 2fr;
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
    color: blueviolet;
    word-wrap: break-word;
    text-align: justify;
}
</style>