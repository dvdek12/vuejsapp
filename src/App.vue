<template>
  <div id="app" class="w-screen h-screen flex justify-center">

    
      <!-- Navbar -->
      <div class="hidden lg:flex absolute w-auto h-auto top-0 frozen-bg-effect py-5 px-24 rounded-b-lg md:rounded-b-full z-20">
        <div class="flex flex-col space-y-8 items-center justify-center">

            <div class="grid grid-cols-2 gap-4 md:flex md:space-y-0 md:flex-wrap md:flex-row justify-center space-x-3  z-10" >
              <div @click="getApiData(icon.title)" v-for="icon in icons" :key="icon" class="inline-flex items-center space-x-1 p-2 custom-gradient transition hover:text-white rounded-xl cursor-pointer">
                  <img :src="icon.src" class="w-6 h-6" alt="">
                  <p class="font-semibold">
                    {{icon.title}}
                  </p>
              </div>
            </div>

            <div class="inline-flex items-center space-x-6">
              <div class="inline-flex space-x-1 items-center">
                <input type="text" class="bg-gray-400 px-4 py-2 text-white font-base rounded-full 
                placeholder-white placeholder-opacity-50 focus:outline-none" 
                placeholder="Search some news..." v-model="searchInput">

                <button @click="getApiData('')">
                  <img :src="searchIcon" class="w-5 h-5">
                </button>
              </div>

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

        </div>
      </div>
    
    <!-- Mobile Menu Toggler Hamburger -->
    <div class="flex lg:hidden absolute w-screen justify-end top-0 p-12">
      <button class="mobile-menu-button focus:te" onclick="this.classList.toggle('opened');this.setAttribute('aria-expanded', this.classList.contains('opened'))">
        <!-- <img :src="hamburgerIcon" class="w-10 h-10"> -->
        <svg width="70" height="70" viewBox="0 0 100 100">
          <path class="line line1" d="M 20,29.000046 H 80.000231 C 80.000231,29.000046 94.498839,28.817352 94.532987,66.711331 94.543142,77.980673 90.966081,81.670246 85.259173,81.668997 79.552261,81.667751 75.000211,74.999942 75.000211,74.999942 L 25.000021,25.000058" />
          <path class="line line2" d="M 20,50 H 80" />
          <path class="line line3" d="M 20,70.999954 H 80.000231 C 80.000231,70.999954 94.498839,71.182648 94.532987,33.288669 94.543142,22.019327 90.966081,18.329754 85.259173,18.331003 79.552261,18.332249 75.000211,25.000058 75.000211,25.000058 L 25.000021,74.999942" />
        </svg>
      </button>
    </div>

    <!-- Mobile Menu Navbar -->
    <div class="flex lg:hidden absolute sidebar frozen-bg-effect left-0 top-0 w-64 h-auto z-20 rounded-r-2xl transform -translate-x-full transition duration-200 ease-in-out lg:relative lg:translate-x-0">
      <div class="flex flex-col space-y-3 p-8">

        <div @click="getApiData(icon.title)" v-for="icon in icons" :key="icon" class="inline-flex items-center space-x-1 p-2 custom-gradient transition hover:text-white rounded-xl cursor-pointer">
          <img :src="icon.src" class="w-6 h-6" alt="">
          <p class="font-semibold">
             {{icon.title}}
          </p>
        </div>
        
        <div class="flex-col items-center space-y-6 pt-12">
          <div class="inline-flex space-x-1 items-center">
            <input type="text" class="bg-gray-400 px-4 py-2 text-white w-full font-base rounded-full 
                placeholder-white placeholder-opacity-50 focus:outline-none" 
                placeholder="Search some news..." v-model="searchInput">

            <button @click="getApiData()">
              <img :src="searchIcon" class="w-5 h-5">
            </button>
          </div>

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
        
      </div>
    </div>
  
    <div class="flex flex-col space-y-6 items-center mt-40">
      <!-- <div class="flex flex-col space-y-6 w-96">
        <div class="inline-flex space-x-16 items-center">
          <input type="text" class="w-auto h-auto p-2 rounded-md focus:outline-none ring-4 ring-indigo-900 ring-offset-2 ring-offset-indigo-500" 
            placeholder="Search some news..."
            v-model="search">

            

            
        </div>

        
        <button class="w-auto h-auto p-5 bg-indigo-600 text-white rounded-xl">
          Click To Get News!
        </button>
      </div> -->

      <div v-if="show" class="grid p-4 lg:p-2 grid-cols-1 gap-20 md:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4">
          <div v-for="news in arrayOfNews" :key="news" data-aos="zoom-in-down">
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
import search from './assets/icons/search.png'
import apple from './assets/icons/apple.png'
import android from './assets/icons/android.png'
import basketball from './assets/icons/basketball-ball.png'
import covid from './assets/icons/coronavirus.png'
import trophy from './assets/icons/trophy.png'
import film from './assets/icons/film-slate.png'
import car from './assets/icons/electric-car.png'
import metaverse from './assets/icons/metaverse.png'
import hamburger from './assets/icons/hamburger.png'



export default {
  name: 'App',
  data() {
    return {
        arrayOfNews: [],
        searchInput: '',
        language: 'English',
        show: false,
        noResults: false,
        searchIcon: search,
        hamburgerIcon: hamburger,
        isPicked: false,
        icons: [
          {
            src: apple,
            title: 'Apple'
          },
          {
            src: android,
            title: 'Android'
          },
          {
            src: basketball,
            title: 'Basketball'
          },
          {
            src: covid,
            title: 'COVID-19'
          },
          {
            src: trophy,
            title: 'Football'
          },
          {
            src: car,
            title: 'Tesla'
          },
          {
            src: film,
            title: 'Films'
          },
          {
            src: metaverse,
            title: 'Metaverse'
          }
        ]
    }
  },

  mounted(){
    toggleSidebar()
  },

  methods: {
    getApiData(search) {
      if(search.length === 0){
        search = this.searchInput
      }
        var url = 'https://newsapi.org/v2/everything?' +
          'q='+ search.toLowerCase() +'&' +
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
        this.isPicked = true;
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
    },
    
    
  },
  components: {
    News
  }
}


function toggleSidebar(){
      const btn = document.querySelector('.mobile-menu-button');
      const sidebar = document.querySelector('.sidebar');

      btn.addEventListener('click', () => {
        sidebar.classList.toggle('-translate-x-full');
      })
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

.frozen-bg-effect {
  background-color: rgba(255, 255, 255, 0.1);
  box-shadow: 0 0 1rem 0 rgba(0, 0, 0, .2); 
  backdrop-filter: blur(10px);
}

.custom-gradient {
  background: rgb(200,198,230);
background: linear-gradient(90deg, rgba(200,198,230,1) 19%, rgba(236,216,216,1) 30%, rgba(194,192,212,1) 47%, rgba(209,245,239,1) 64%, rgba(245,247,229,1) 85%, rgba(170,226,237,1) 100%);
}


/* Hamburger Animation */
.line {
  fill: none;
  stroke: black;
  stroke-width: 6;
  transition: stroke-dasharray 600ms cubic-bezier(0.4, 0, 0.2, 1),
    stroke-dashoffset 600ms cubic-bezier(0.4, 0, 0.2, 1);
}
.line1 {
  stroke-dasharray: 60 207;
  stroke-width: 6;
}
.line2 {
  stroke-dasharray: 60 60;
  stroke-width: 6;
}
.line3 {
  stroke-dasharray: 60 207;
  stroke-width: 6;
}
.opened .line1 {
  stroke-dasharray: 90 207;
  stroke-dashoffset: -134;
  stroke-width: 6;
}
.opened .line2 {
  stroke-dasharray: 1 60;
  stroke-dashoffset: -30;
  stroke-width: 6;
}
.opened .line3 {
  stroke-dasharray: 90 207;
  stroke-dashoffset: -134;
  stroke-width: 6;
}
</style>
