<template>
  <div id="wordcloud"></div>
</template>

<script>
import * as d3 from "d3";
import cloud from "d3-cloud";
import { onMounted } from "@vue/runtime-core";

export default {
  setup() {
    const myWords = [
      { word: "Running", size: "10" },
      { word: "Surfing", size: "20" },
      { word: "Climbing", size: "50" },
      { word: "Kiting", size: "30" },
      { word: "Sailing", size: "20" },
      { word: "Snowboarding", size: "60" },
    ];

    const margin = { top: 10, right: 10, bottom: 10, left: 10 },
      width = 450 - margin.left - margin.right,
      height = 450 - margin.top - margin.bottom;

    onMounted(() => {
      const svg = d3
        .select("#wordcloud")
        .append("svg")
        .attr("width", width + margin.left + margin.right)
        .attr("height", height + margin.top + margin.bottom)
        .append("g")
        .attr("transform", `translate(${margin.left}, ${margin.top})`);

      const layout = cloud()
        .size([width, height])
        .words(
          myWords.map(function(d) {
            return { text: d.word, size: d.size };
          })
        )
        .padding(5)
        .rotate(function() {
          return ~~(Math.random() * 2) * 90;
        })
        .fontSize(function(d) {
          return d.size;
        })
        .on("end", draw);
      layout.start();

      function draw(words) {
        svg
          .append("g")
          .attr(
            "transform",
            `translate(${layout.size()[0] / 2}, ${layout.size()[1] / 2})`
          )
          .selectAll("text")
          .data(words)
          .enter()
          .append("text")
          .style("font-size", function(d) {
            return `${d.size}px`;
          })
          .style("fill", "#69b3a2")
          .attr("text-anchor", "middle")
          .style("font-family", "Impact")
          .attr("transform", function(d) {
            console.log(d);
            return `translate(${[d.x, d.y]})rotate(${d.rotate})`;
          })
          .text(function(d) {
            return d.text;
          });
      }
    });
  },
};
</script>

<style></style>
