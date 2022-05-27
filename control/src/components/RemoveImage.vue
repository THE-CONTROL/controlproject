<template>
  <div class="w-full h-full bg-yellow absolute pt-20">
        <div class="bg-gray-900 m-auto w-60 sm:w-72 md:w-80 lg:w-96 rounded-3xl 
         text-gray-100 pb-4">
            <div>
                <button @click="showModalfunc2" class="p-1 w-8 bg-red-600 
                hover:bg-red-800 font-bold text-base float-right rounded-tr-3xl">X</button>
                <h1 class="font-bold text-2xl text-center ml-3 w-full">Remove Image</h1>
                <div class="p-1 m-auto border-2 
                border-gray-100 mt-4 rounded-lg text-center mx-4">
                    <h2 class="w-full text-base mr-1">Are you sure you want to delete this image?</h2>
                    <div class="w-full">
                        <img :src="newImage" class="h-20 w-20 m-auto mt-4 rounded-lg object-scale-down" />
                        <div class="w-full mt-4 text-sm mb-4">
                            <button @click="removeSlide" type="submit" 
                            class="w-12 mr-4 rounded-lg p-1 bg-green-600 
                            hover:bg-green-800">Yes</button>
                            <button @click="showModalfunc2" type="submit" class="w-12 
                            rounded-lg p-1 bg-red-600 hover:bg-red-800">No</button>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div> 
</template>

<script>
export default {
    name: "RemoveImage",
    props: {
        newImage: { type: String },
        removeSlide: { type: Function },
        showModalfunc2: { type: Function }
    }
}
</script>

<style>

</style>