<template>
  <wordcloud />
  <heatmap />
  <circular-packing />
</template>

<script>
import CircularPacking from "./components/CircularPacking.vue";
import Heatmap from "./components/Heatmap.vue";
import Wordcloud from "./components/Wordcloud.vue";

export default {
  name: "App",
  components: { Wordcloud, Heatmap, CircularPacking },
};
</script>

<style></style>
