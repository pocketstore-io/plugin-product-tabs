<template>
  <section class="grid grid-cols-6 gap-3">
    <div class="col-span-6 md:col-span-3" v-html="block.content" v-for="block in blocks"></div>
  </section>
</template>

<script lang="ts" setup>
const pb = usePocketBase();
const blocks = ref([]);
const props = defineProps({
  product: {type: String, required: true}
})

watch(() => props.product, async () => {
  blocks.value = (await pb.collection('product_blocks').getList(1, 10)).items;
})
</script>
