<template>
  <div class="w-full h-full bg-yellow absolute pt-20">
        <div class="bg-gray-900 m-auto w-60 sm:w-72 md:w-80 lg:w-96 rounded-3xl 
         text-gray-100 pb-4">
            <div>
                <button @click="showModalfunc0" class="p-1 w-8 bg-red-600 hover:bg-red-800 
                font-bold text-base float-right rounded-tr-3xl">X</button>
            </div>
            <form @submit.prevent="emitEvent(image)" class="w-full font-serif">
                <h1 class="font-bold text-2xl text-center w-full ml-3">Add Image</h1>
                <div class="m-auto border-2 border-gray-100 mx-4 px-2 py-4 mt-4 rounded-lg">
                    <section class="w-full">
                        <label for="slide" class="w-full text-base mr-1">Image:</label>
                        <input type="file" @change="onFileChange"  
                         class="m-auto w-3/4 overflow-hidden text-base">
                    </section> 
                    <div v-if="image" class="w-full">
                        <img :src="image" class="h-20 w-20 m-auto mt-4 rounded-lg object-scale-down" />
                    </div>
                    <div v-if="image" class="flex flex-wrap w-full mt-4">
                        <button type="submit" class="w-14 rounded-lg p-1 mx-auto bg-green-600 
                        hover:bg-green-800">Add</button>
                    </div> 
                </div>
            </form>
        </div>
    </div> 
</template>

<script>
export default {
    name: "ImageMod",
    props: {
        showModalfunc0: { type: Function }, 
        addImagelistener: { type: Function },
        },
    data() {
        return {
            image: null,
            imageArray: ["jpg", "png", "jpeg"]
        } 
    },
    methods: {
        onFileChange(e) {
            var files = e.target.files || e.dataTransfer.files
            if (!files.length) {return}
            this.createImage(files[0])
        },

        createImage(file) {
            this.image = new Image()
            var reader = new FileReader()
            const check = file.name.toString()
            const imageType = check.split(".")[1]
            if (this.imageArray.includes(imageType)) {
                reader.onload = (e) => {
                    this.image = e.target.result
                };
                reader.readAsDataURL(file)
            }
            else {
                alert("Only .jpg, .jpeg and .png files are allowed")
                this.image = null
            }
        },

        emitEvent(value) {
            this.$emit('imageAdded', value)
        }
    }
}
</script>