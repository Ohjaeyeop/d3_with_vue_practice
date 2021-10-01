<template>
  <wordcloud />
  <heatmap />
</template>

<script>
import Heatmap from "./components/Heatmap.vue";
import Wordcloud from "./components/Wordcloud.vue";

export default {
  name: "App",
  components: { Wordcloud, Heatmap },
};
</script>

<style></style>
