<template>
  <div>
    <l-map
      @click="addMarker"
      style="height: 100vh;z-index:1"
      :zoom="zoom"
      :center="center"
    >
      <l-tile-layer :url="url"></l-tile-layer>
      <l-marker
        :title="marker.icon.description"
        v-for="(marker, i) in markers"
        :key="i"
        :lat-lng="[marker.latlng.lat, marker.latlng.lng]"
      >
        <l-icon
          :icon-size="marker.icon.iconSize"
          :icon-anchor="marker.icon.anchor"
          :icon-url="marker.icon.src"
        />
      </l-marker>
    </l-map>
    <marker-select :markers="markers" @change="setChanges" />
  </div>
</template>

<script>
import { LMap, LTileLayer, LMarker, LIcon } from "vue2-leaflet";
import MarkerSelect from "./components/MarkerSelect.vue";

export default {
  name: "app",
  components: {
    LMap,
    LTileLayer,
    LMarker,
    MarkerSelect,
    LIcon,
  },
  data() {
    return {
      markers: [],
      url: "https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png",
      center: [47.41322, -1.219482],
      zoom: 13,
      iconStyle: { iconType: 1, iconSize: ["S", 10], iconColor: "brown" },
      staticAnchor: [16, 37],
      customText: "Foobar",
      iconSize: 64,
    };
  },
  computed: {
    dynamicSize() {
      return [this.iconStyle.iconSize[1], this.iconStyle.iconSize[1] * 1.15];
    },
    dynamicAnchor() {
      return [
        this.iconStyle.iconSize[1] / 2,
        this.iconStyle.iconSize[1] * 1.15,
      ];
    },
    dynamicType() {
      return this.images[this.iconStyle.iconType - 1];
    },
  },
  methods: {
    addMarker(el) {
      let name = prompt("Write a description for this marker", "Marker");
      this.markers.push({
        latlng: el.latlng,
        icon: {
          iconSize: [
            this.iconStyle.iconSize[1],
            this.iconStyle.iconSize[1] * 1.15,
          ],
          anchor: [
            this.iconStyle.iconSize[1] / 2,
            this.iconStyle.iconSize[1] * 1.15,
          ],
          src: require(`./assets/${this.iconStyle.iconColor}${this.iconStyle.iconType}.png`),
          name,
          description: "",
          edit: false,
        },
      });
      localStorage.setItem("Markers", JSON.stringify(this.markers));
    },
    setChanges(iconProps) {
      this.iconStyle = iconProps;
    },
  },
  created() {
    this.markers = JSON.parse(localStorage.getItem("Markers"));
  },
};
</script>

<style>
.toggle {
  display: none;
}
.btn-group label {
  background-color: orange;
  margin: 10px;
}
#mapid {
  height: 100vh;
  z-index: 1;
}
</style>
