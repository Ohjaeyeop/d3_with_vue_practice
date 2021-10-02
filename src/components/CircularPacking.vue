<template>
  <div id="circular_packing"></div>
</template>

<script>
import * as d3 from "d3";
import { onMounted } from "@vue/runtime-core";

export default {
  setup() {
    const width = 460;
    const height = 460;

    const color = d3
      .scaleOrdinal()
      .domain(["Asia", "Europe", "Africa", "Oceania", "Americas"])
      .range(d3.schemeSet1);

    const size = d3
      .scaleLinear()
      .domain([0, 1400000000])
      .range([7, 55]);

    onMounted(() => {
      const svg = d3
        .select("#circular_packing")
        .append("svg")
        .attr("width", width)
        .attr("height", height);

      d3.csv(
        "https://raw.githubusercontent.com/holtzy/data_to_viz/master/Example_dataset/11_SevCatOneNumNestedOneObsPerGroup.csv"
      ).then(function(data) {
        data = data.filter(function(d) {
          return d.value > 10000000;
        });

        const Tooltip = d3
          .select("#circular_packing")
          .append("div")
          .style("opacity", 0)
          .attr("class", "tooltip")
          .style("background-color", "white")
          .style("border", "solid")
          .style("border-radius", "5px")
          .style("padding", "5px")
          .style("position", "absolute");

        const mouseover = function(event, d) {
          Tooltip.style("opacity", 1);
        };

        const mousemove = function(event, d) {
          Tooltip.html(
            "<u>" + d.key + "</u>" + "<br>" + d.value + " inhabitants"
          )
            .style("left", event.x + 20 + "px")
            .style("top", event.y - 30 + "px");
        };

        const mouseleave = function(event, d) {
          Tooltip.style("opacity", 0);
        };

        const node = svg
          .append("g")
          .selectAll("circle")
          .data(data)
          .join("circle")
          .attr("class", "node")
          .attr("r", (d) => size(d.value))
          .attr("cx", width / 2)
          .attr("cy", height / 2)
          .style("fill", (d) => color(d.region))
          .style("fill-opacity", 0.8)
          .attr("stroke", "black")
          .style("stroke-width", 1)
          .on("mouseover", mouseover)
          .on("mousemove", mousemove)
          .on("mouseleave", mouseleave);

        const simulation = d3
          .forceSimulation()
          .force(
            "center",
            d3
              .forceCenter()
              .x(width / 2)
              .y(height / 2)
          )
          .force("charge", d3.forceManyBody().strength(0.1))
          .force(
            "collide",
            d3
              .forceCollide()
              .strength(0.2)
              .radius(function(d) {
                return size(d.value) + 3;
              })
              .iterations(1)
          );

        simulation.nodes(data).on("tick", function(d) {
          node.attr("cx", (d) => d.x).attr("cy", (d) => d.y);
        });
      });
    });
  },
};
</script>

<style></style>
