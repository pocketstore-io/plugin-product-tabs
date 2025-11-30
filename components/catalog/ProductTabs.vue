<template>
  <div class="tabs tabs-border bg-red-400 mb-3">
    <input
        type="radio"
        name="my_tabs_2"
        class="tab"
        @click="selected = 'description'"
        :checked="selected == 'description'"
        aria-label="Beschreibung"
    />
    <div class="tab-content bg-base-100 p-5 md:p-10">
      <CatalogTabDescription/>
    </div>

    <input
        type="radio"
        name="my_tabs_2"
        class="tab"
        @click="selected = 'pictures'"
        :checked="selected == 'pictures'"
        aria-label="Fotos"
    />
    <div class="tab-content bg-base-100 p-10">
      <ImageSlider :images="images"></ImageSlider>
    </div>

    <input
        type="radio"
        name="my_tabs_2"
        class="tab"
        aria-label="Reviews"
        :checked="selected == 'reviews'"
        @click="selected = 'reviews'"
    />
    <div class="tab-content border-base-300 bg-base-100 p-10">
      <Review
          v-for="review in reviews"
          :key="review.id"
          :identifier="review.id"
      />
      <CatalogReviewForm/>
    </div>

    <input
        type="radio"
        name="my_tabs_2"
        class="tab"
        @click="selected = 'similiar'"
        aria-label="Ähnliche Produkte"
        :checked="selected == 'similiar'"
    />
    <div class="tab-content border-base-300 bg-base-100 p-10">
      Tab content 3
    </div>

    <input
        type="radio"
        name="my_tabs_2"
        class="tab"
        @click="selected = 'equal'"
        aria-label="Dazu passende Produkte"
        :checked="selected == 'equal'"
    />
    <div class="tab-content border-base-300 bg-base-100 p-10">
      Tab content 4
    </div>
  </div>
</template>
<script lang="ts" setup>
import {useRoute} from "vue-router";
import Review from "../catalog/Review.vue";
import {usePocketBase} from "~/utils/pocketbase";
import ImageSlider from "~/components/ImageSlider.vue";

const pb = usePocketBase();
const route = useRoute();

const props = defineProps({
  identifier: {type: String, required: true},
});

const reviews = ref([]);

const selected = ref("description");
const images = [
  {media: 'https://image.tmdb.org/t/p/original/ah3m5E28pE4vsFnNvoTTdVxxT3B.jpg'},
  {media: 'https://image.tmdb.org/t/p/original/6U17IFhOo7omnuD2mrFQIaHx82B.jpg'},
  {media: 'https://image.tmdb.org/t/p/original/ebg8TF2vYaSVNVLv8WO8omebrjw.jpg'},
]

onMounted(async () => {
  reviews.value = (
      await pb.collection("reviews").getList(1, 9, {
        filter: 'product="' + route.params.slug.replace(".html", "") + '"',
      })
  ).items;
});
</script>
