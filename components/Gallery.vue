<template >
  <div class="container mx-auto">
    <h1 class="text-center text-4xl font-bold my-5">All images</h1>
    <div class="flex justify-center" v-if="isloading">
      <div class="lds-hourglass"></div>
    </div>
    <input
      type="text"
      name=""
      id=""
      class="rounded border w-full py-2 px-2 text-xl outline-green-400 my-4"
      @keypress.enter="$fetch"
      v-model="search"
    />
    <div v-if="search === null">
      <div
        class="
          grid grid-cols-1
          md:grid-cols-2
          lg:grid-cols-3
          xl:grid-cols-3
          gap-3
          mx-4
        "
      >
        <div v-for="(image, index) in images" :key="index">
          <img class="w-full rounded-lg" :src="image.urls.full" alt="" />
        </div>
      </div>
    </div>
    <div else>
      <div
        class="
          grid grid-cols-1
          md:grid-cols-2
          lg:grid-cols-3
          xl:grid-cols-3
          gap-3
          mx-4
        "
      >
        <div v-for="(image, index) in searchImage" :key="index">
          <img class="w-full rounded-lg" :src="image.urls.full" alt="" />
        </div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: "GalleryComp",
  data: () => ({
    images: [],
    searchImage: [],
    isloading: false,
    search: null,
  }),

  methods: {
    async getImages() {
      this.isloading = true;

      const images = await this.$axios.get(
        "https://api.unsplash.com/photos?client_id=?"
      );
      this.images = images.data;
      this.isloading = false;
    },
    async searchImages() {
      this.isloading = true;

      const images = await this.$axios.get(
        `https://api.unsplash.com/search?query=${this.search}&client_id=?`
      );
      this.searchImage = images.data.photos.results;
      this.search = "";
      this.isloading = false;
    },
  },

  async fetch() {
    if (this.search === null) {
      await this.getImages();
    } else {
      await this.searchImages();
    }
  },
};
</script>
<style>
</style>
