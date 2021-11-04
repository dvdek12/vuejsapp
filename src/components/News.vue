<template>
    <div class="flex flex-col justify-center  w-64 bg-white rounded-lg" style="height: 30rem">
        <div class="w-full h-1/3">
            <img :src="news.urlToImage" alt="" class="w-full h-full rounded-lg">
        </div>
        <div class="relative w-full h-full flex flex-col items-between space-y-4">

            <div class="flex flex-col items-start p-5 w-full h-auto space-y-1">
                <p class="text-gray-400 text-sm font-bold">
                  {{news.publishedAt.slice(0,10)}}
                </p>

                <h3 class="font-bold text-xl text-gray-900 text-left">
                  {{this.reduceTitle(news.title)}}
                </h3>

                <p class="text-gray-700 text-xs font-semibold text-left space-y-10 leading-loose">
                  {{this.reduceDescription(news.description)}}
                </p>
            
                
            </div>

            <div class="absolute -inset-x-12 bottom-0 w-full h-auto ">
              <button class="text-purple-700  my-6 text-xl font-semibold">
                  <a :href="news.url" target="_blank">Find out more</a>
              </button>
            </div> 

        </div>
    </div>
</template>

<script>
export default {
  name: 'News',
  props: ['news'],
  methods: {
    reduceTitle(title) {
        if(typeof String && title.length > 15){
          return title.slice(0,30) + "..."
        }
    },

    reduceDescription(desc) {
      return desc.slice(0, (desc.indexOf(".") + 1) || (desc.indexOf("?") + 1))
    }
    
  }
}


</script>


