<template>
    <div class="flex flex-col justify-center  w-96 bg-white rounded-lg" style="height: 45rem">
        <div class="w-full h-1/3">
            <img :src="news.urlToImage" alt="" class="w-full h-full rounded-lg">
        </div>
        <div class="relative w-full h-full flex flex-col items-between space-y-4">

            <div class="flex flex-col items-start p-5 w-full h-auto space-y-6">
                <p class="text-gray-400 text-sm font-bold">
                  {{news.publishedAt.slice(0,10)}}
                </p>

                <h3 class="font-bold text-3xl text-gray-900 text-left">
                  {{this.reduceTitle(news.title)}}
                </h3>

                <p class="text-gray-700 text-sm font-semibold text-left space-y-10 leading-loose">
                  {{news.description}}
                </p>
            
                
            </div>

            <div class="absolute -inset-x-24 bottom-0 w-full h-auto ">
              <button class="text-purple-700  my-12 text-2xl font-semibold">
                  Find out more
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
          return title.slice(0,50) + "..."
        }
    }

    
  }
}


</script>


