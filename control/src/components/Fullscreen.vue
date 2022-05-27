<template>
  <div class="absolute h-screen w-full bg-black opacity-80"></div>
  <div class="">
      <div v-for="(image, index) in images" :key="index" 
        class="flex flex-wrap m-auto relative bg-slate-500">
            <transition name="fade">
                <div v-if="count == index">
                    <img :src="image" alt="Photo" 
                    class="absolute object-contain rounded-2xl w-screen h-screen">
                    <div class="w-full absolute text-xs font-bold text-gray-100 mt-14">
                        <button @click="showModalfunc1(image, index)" class="bg-red-600 
                        rounded-2xl 
                        hover:bg-red-800 p-1 float-right">Delete</button>
                        <button @click="setFullscreen(index)" class="bg-gray-600 
                        rounded-2xl 
                        hover:bg-gray-800 p-1 float-left">Exit</button>
                    </div>
                </div>
            </transition>
          <div class="w-full absolute font-extrabold text-center text-2xl mt-80">
              <div @click="prev()" class="bg-gray-700 w-5 h-10 rounded-3xl float-left 
              hover:bg-gray-500">&lt;</div>
              <div @click="next()" class="bg-gray-700 w-5 h-10 rounded-3xl float-right 
              hover:bg-gray-500">&gt;</div>
          </div>
      </div>
      <div class="w-full absolute text-xs font-bold text-gray-100 bottom-10">
          <button @click="showModalfunc()" class="text-teal-100 bg-teal-600
            rounded-2xl hover:bg-teal-800 p-1 float-right mt-2">Add Image</button>
          <button v-if="!slideShow" @click="togSlide()" class="bg-gray-600 rounded-2xl 
          hover:bg-gray-800 p-1 float-left mt-2">Slideshow</button>
          <button v-if="slideShow" @click="togSlide()" class="bg-gray-600 rounded-2xl 
          hover:bg-gray-800 p-1 float-left mt-2">Stop slide</button>
      </div>
  </div>
</template>

<script>
export default {
    name: "FullScreen",
    props: { 
        setFullscreen: { type: Function },
        images: { type: Array },
        count: { type: Number },
        prev: { type: Function },
        next: { type: Function },
        togSlide: { type: Function },
        showModalfunc: { type: Function },
        showModalfunc1: { type: Function },
        slideShow: { type: Boolean }
     },
}
</script>

<style>

</style>

 