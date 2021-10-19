<template>
  <div id="app" class="w-screen h-screen flex justify-center">

    <div class="flex flex-col space-y-6 items-center">
      <div class="flex flex-col space-y-6 w-96">
        <div class="inline-flex space-x-4 items-center">
          <input type="text" class="w-auto h-auto p-2 rounded-md focus:outline-none ring-4 ring-indigo-900 ring-offset-2 ring-offset-indigo-500" 
            placeholder="Search some news..."
            v-model="search">

            

            <select v-model="language" class="focus:outline-none w-auto h-6 rounded-lg ring-2 ring-indigo-900 ring-offset-2 ring-offset-indigo-500">
              <option class="text-black font-semibold">Arabic</option>
              <option class="text-black font-semibold">German</option>
              <option class="text-black font-semibold">English</option>
              <option class="text-black font-semibold">Spanish</option>
              <option class="text-black font-semibold">French</option>
              <option class="text-black font-semibold">Italian</option>
              <option class="text-black font-semibold">Russian</option>
            </select>
        </div>

        
        <button @click="getApiData()" class="w-auto h-auto p-5 bg-indigo-600 text-white rounded-xl">
          Click To Get News!
        </button>
      </div>

      <div v-if="show" class="grid grid-cols-1 gap-10 md:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4">
          <div v-for="news in arrayOfNews" :key="news">
            <News :news="news"/>
          </div>
      </div>

      <div v-if="noResults" class="w-auto p-5">
          <p class="text-white fonmt-bold text-2xl">Sorry, We didn't find typed phrase..</p>
      </div>
    </div>

  </div>
</template>

<script>
import News from './components/News.vue'

export default {
  name: 'App',
  data() {
    return {
        arrayOfNews: [],
        search: '',
        language: '',
        show: false,
        noResults: false
    }
  },
  methods: {
    getApiData() {
        var url = 'https://newsapi.org/v2/everything?' +
          'q='+ this.search +'&' +
          'from=2021-10-17&' +
          'sortBy=popularity&' +
          'language='+ this.convertLanguagesToISO(this.language) + '&' + 
          'apiKey=6f7553412aea42819cde65451edff8b6';
        
        var req = new Request(url);

        const news = fetch(req).then(function(response) {
          var res = response.json();
          return res;
        })

        const showNews = () => {
          news.then((res) => {
            if(res.articles.length !== 0) {
              this.noResults = !this.noResults
              this.arrayOfNews = res.articles;
            }else{
              this.show = false
              this.noResults = true
            }
          });
        };

        showNews()
        this.show = true;
    },

    convertLanguagesToISO(language) {
        switch(language) {
          case "Arabic":
            return "ar";
          case "German":
            return "de";
          case "English":
            return "en";
          case "Spanish":
            return "es";
          case "French":
            return "fr";
          case "Italian":
            return "it";
          case "Russian":
            return "ru";
        }
    }
  },
  components: {
    News
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
