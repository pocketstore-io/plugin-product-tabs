<template>
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
</template>
<script setup lang="ts">
import {usePocketBase} from "~/utils/pocketbase";
import ImageSlider from "~/components/content/ImageSlider.vue";

const props = defineProps({
  product: {required: true, type: String}
});
const pb = usePocketBase();
const images = ref([]);

watch(() => props.product, async () => {
  images.value = await pb.collection('product_pictures').getFullList({
    filter: 'product="' + props.product + '"'
  })
});
</script>