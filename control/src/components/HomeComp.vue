<template>
    <div v-if="!fullscreen && (!showModal && !showModal1)" class="py-4 w-full absolute h-full bottom-0 pt-20">
        <div v-if="!images.length" class="w-full absolute text-center text-teal-100 text-base top-32">
            <p class="pb-2 text-xl">There are no images in your gallery</p>
            <button @click="showModalfunc()" class="bg-teal-600 rounded-3xl 
            hover:bg-teal-800 p-1 m-auto">Add images</button>
        </div>
        <div v-for="(image, index) in images" :key="index" 
        class="flex flex-wrap m-auto relative bg-slate-500 w-11/12">
            <transition name="fade">
                <div v-if="count == index">
                    <img :src="image" alt="Photo" 
                    class="w-full absolute object-cover rounded-2xl h-80">
                    <div class="w-full absolute text-xs font-bold text-gray-100 ">
                        <button @click="showModalfunc1(image, index)" class="bg-red-600 
                        rounded-tr-2xl 
                        hover:bg-red-800 p-1 float-right">Delete</button>
                        <button @click="setFullscreen(index)" class="bg-gray-600 
                        rounded-tl-2xl 
                        hover:bg-gray-800 p-1 float-left">Full</button>
                    </div>
                    <div class="w-full absolute mt-72 text-xs font-bold text-gray-100">
                        <button @click="showModalfunc()" class="text-teal-100 bg-teal-600
                        rounded-br-2xl hover:bg-teal-800 p-1 float-right mt-2">Add Image</button>
                        <button v-if="!slideShow" @click="togSlide()" class="bg-gray-600 
                        rounded-bl-2xl 
                        hover:bg-gray-800 p-1 float-left mt-2">Slideshow</button>
                        <button v-if="slideShow" @click="togSlide()" class="bg-gray-600 
                        rounded-bl-2xl 
                        hover:bg-gray-800 p-1 float-left mt-2">Stop slide</button>
                    </div>
                </div>
            </transition>
            <div class="w-full mt-36 absolute font-extrabold text-center text-2xl">
                <div @click="prev()" class="bg-gray-700 w-5 h-10 rounded-3xl float-left 
                hover:bg-gray-500">&lt;</div>
                <div @click="next()" class="bg-gray-700 w-5 h-10 rounded-3xl float-right 
                hover:bg-gray-500">&gt;</div>
            </div>
            <div class="absolute top-80 flex flex-wrap justify-center w-full pb-10">
                <img @click="curImg(index)" v-for="(item, index) in images" 
                :key="index" :src="item" :alt="index" class="mt-4 object-cover hover:opacity-50 mx-0.5 
                rounded-lg w-1/5 h-32"
                :class="index == count ? 'border-8 border-gray-500':''">
            </div>
        </div>
    </div>

    <transition>
        <FullScreen v-if="fullscreen" :fullscreen="fullscreen" :setFullscreen="setFullscreen"
        :images="images" :count="count" :prev="prev" :showModalfunc1="showModalfunc1"
        :next="next" :togSlide="togSlide" :showModalfunc="showModalfunc" :slideShow="slideShow"/>
    </transition>
    <transition name="fade">
        <div v-if="showModal" class="absolute bottom-0 w-full h-full">
            <div class="absolute w-full h-screen bg-black opacity-70">
            </div>
            <div>
                <ImageMod :showModalfunc0="showModalfunc0" :addImagelistener="addImagelistener" 
                @imageAdded="addImagelistener"/>
            </div>
        </div>
    </transition>
    <transition name="fade">
        <div v-if="showModal1" class="absolute bottom-0 w-full h-full">
            <div class="absolute w-full h-full bg-black opacity-70">
            </div>
            <div>
                <RemoveImage :newImage="newImage"  
                :removeSlide="removeSlide" :showModalfunc2="showModalfunc2" />
            </div>
        </div>
    </transition>
</template>

<script>
import { onBeforeUnmount, onMounted, ref } from "vue"
import ImageMod from "./ImageMod.vue"
import RemoveImage from "./RemoveImage.vue"
import FullScreen from "./Fullscreen.vue"

export default {
    name: "HomeComp",
    components: {
        ImageMod,
        RemoveImage,
        FullScreen
    },
    setup() {
        const count = ref(0)
        const images = ref([  ])
        const interval = ref("")
        const showModal = ref(false)
        const showModal1 = ref(false)
        const newImage = ref("")
        const imageId = ref("")
        const fullscreen = ref(false)
        const slideShow = ref(false)

        function slide() {
            if (slideShow.value == true) {
                debounce()
                if (count.value == (images.value.length - 1)) {
                    count.value = 0
                }
                else {
                    count.value++
                }
            }
        }

        function curImg(index) {
            count.value = index
            debounce()
        }

        function next() {
            if (count.value == (images.value.length - 1)) {
                count.value = 0
            }
            else {
                count.value++
            }  
            debounce()
        }

        function prev() {
            if (count.value == 0) {
                count.value = (images.value.length - 1)
            }
            else {
                count.value--
            }  
            debounce()
        }

        function showModalfunc() {
            showModal.value = true
        }

        function showModalfunc0() {
            showModal.value = false
        }

        function showModalfunc1(image, index) {
            showModal1.value = true
            newImage.value = image
            imageId.value = index
        }

        function showModalfunc2() {
            showModal1.value = false
            newImage.value = ""
            imageId.value = ""
            fullscreen.value = false
        }

        function setFullscreen(index) {
            fullscreen.value = !fullscreen.value
            count.value = index
        }

        function togSlide() {
            slideShow.value = !slideShow.value
        }

        function addImagelistener(value) {
            newImage.value = value
            if (value === null) {
                { alert("Please add an image") }
            }
            else {
                if (images.value.includes(newImage.value)) {
                    alert("Image name already in gallery")
                }
                else {
                    images.value[images.value.length] = newImage.value
                    showModalfunc0()
                }
            }
            count.value = images.value.length - 1
            newImage.value = ""
        }

        function removeSlide() {
            images.value = images.value.filter((image, index) => index != imageId.value)
            count.value = 0
            showModalfunc2()
        }

        function debounce() {
            clearInterval(interval.value)
            interval.value = setInterval(() => {
                slide()
            }, 3000)
        }

        onMounted(() => {
            debounce()
        })

        onBeforeUnmount(() => {
            clearInterval(interval.value)
        })

        return { count, slide, curImg, next, prev, interval, images, 
        showModal, showModalfunc, addImagelistener, newImage, removeSlide, 
        showModal1, showModalfunc1, 
        showModalfunc2, imageId, debounce, fullscreen,
        setFullscreen, slideShow, togSlide, showModalfunc0 }
    }
}
</script>

