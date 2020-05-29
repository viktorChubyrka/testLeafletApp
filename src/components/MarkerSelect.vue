<template>
  <div>
    <div
      :class="[!isActive ? 'off2' : '']"
      style="
          position: absolute;
          top: 0;
          right: 0;
          height: 100%;
          width: 400px;
          background-color: rgb(243, 243, 243);
          z-index: 5;
        "
    >
      <div style="margin: 2%;" class="container">
        <div class="row">
          <div class="col-9">
            <h6><i class="fas fa-pencil-alt"></i> DRAW AND MEASURE</h6>
          </div>

          <div class="col-3 icons">
            <i
              v-if="isActive"
              @click="isActive = !isActive"
              class="fas fa-chevron-down"
            ></i
            ><i
              @click="isActive = !isActive"
              v-else
              class="fas fa-chevron-up"
            ></i
            ><i class="fas fa-calendar-times"></i>
          </div>
        </div>
      </div>
      <div
        :class="[!isActive ? 'off' : '']"
        style="
            width: 100%;
            height: 100%;
            background-color: rgb(233, 230, 230);
            padding: 2%;
          "
        class="container"
      >
        <div class="row">
          <div class="col-3">
            <h4
              style="
                  margin: 0;
                  padding: 10%;
                  background-color: orange;
                  border-top-left-radius: 10px;
                  border-top-right-radius: 10px;
                "
            >
              Point
            </h4>
          </div>
        </div>
        <div class="row">
          <div
            style="
                border: 2px solid orange;
                margin-left: 2%;
                background-color: white;
              "
            class="col-11"
          >
            <div style="margin-top: 2%;" class="row">
              <div class="col-6">
                <h6>POINT TYPE:</h6>
                <div style="display:flex">
                  <div
                    @click="(iconType = 1), sendChanges()"
                    :class="[iconType == 1 ? 'selected' : '']"
                    style="width:30px;height:30px;background-color:orange;border-radius:10px;margin:2px;text-align:center;padding-top:2px"
                  >
                    <svg
                      class="bi bi-caret-down-fill"
                      width="1em"
                      height="1em"
                      viewBox="0 0 16 16"
                      fill="currentColor"
                      xmlns="http://www.w3.org/2000/svg"
                    >
                      <path
                        d="M7.247 11.14L2.451 5.658C1.885 5.013 2.345 4 3.204 4h9.592a1 1 0 0 1 .753 1.659l-4.796 5.48a1 1 0 0 1-1.506 0z"
                      />
                    </svg>
                  </div>
                  <div
                    @click="(iconType = 2), sendChanges()"
                    :class="[iconType == 2 ? 'selected' : '']"
                    style="width:30px;height:30px;background-color:orange;border-radius:10px;margin:2px;text-align:center;padding-top:2px"
                  >
                    <svg
                      class="bi bi-circle-fill"
                      width="1em"
                      height="1em"
                      viewBox="0 0 16 16"
                      fill="currentColor"
                      xmlns="http://www.w3.org/2000/svg"
                    >
                      <circle cx="8" cy="8" r="8" />
                    </svg>
                  </div>
                  <div
                    @click="(iconType = 3), sendChanges()"
                    :class="[iconType == 3 ? 'selected' : '']"
                    style="width:30px;height:30px;background-color:orange;border-radius:10px;margin:2px;text-align:center;padding-top:2px"
                  >
                    <svg
                      class="bi bi-diamond-fill"
                      width="1em"
                      height="1em"
                      viewBox="0 0 16 16"
                      fill="currentColor"
                      xmlns="http://www.w3.org/2000/svg"
                    >
                      <path
                        fill-rule="evenodd"
                        d="M6.95.435c.58-.58 1.52-.58 2.1 0l6.515 6.516c.58.58.58 1.519 0 2.098L9.05 15.565c-.58.58-1.519.58-2.098 0L.435 9.05a1.482 1.482 0 0 1 0-2.098L6.95.435z"
                      />
                    </svg>
                  </div>
                </div>
              </div>
              <div class="col-6">
                <h6>POINT SIZE:</h6>
                <div style="display:flex">
                  <div
                    @click="(iconSize = size), sendChanges()"
                    :class="[iconSize[1] == size[1] ? 'selected' : '']"
                    style="width:30px;height:30px;background-color:orange;border-radius:10px;margin:2px;text-align:center;padding-top:2px"
                    v-for="(size, index) in iconSizes"
                    :key="index"
                  >
                    {{ size[0] }}
                  </div>
                </div>
              </div>
            </div>
            <div class="row">
              <div class="col-12">
                <h6>POINT COLOR:</h6>
                <div style="display:flex">
                  <div
                    @click="(iconColor = color), sendChanges()"
                    :class="[iconColor == color ? 'selected' : '']"
                    style="width:10%;height:10%;background-color:orange;border-radius:10px;margin:2px"
                    v-for="(color, index) in iconColors"
                    :key="index"
                  >
                    <div
                      :style="
                        `width:25px;height:25px;background-color:${color};margin: 15% auto;`
                      "
                    ></div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
        <div style="margin-top:2%">
          <ul class="list-group">
            <li class="list-group-item" v-for="(marker, i) in markers" :key="i">
              <div class="row">
                <div v-if="!marker.icon.edit" class="col-9">
                  {{ i + 1 }}. {{ marker.icon.name }}
                </div>
                <div v-else>
                  <input
                    :placeholder="marker.icon.name"
                    v-model="editNameField"
                    type="text"
                  />
                  <button @click="editName(i)">Save</button>
                </div>
                <div class="col-3 icons">
                  <i
                    @click="marker.icon.edit = !marker.icon.edit"
                    class="fas fa-pencil-alt"
                  ></i>
                  <i @click="deleteMarker(i)" class="far fa-trash-alt"></i>
                </div>
              </div>
            </li>
          </ul>
          <ul></ul>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ["markers"],
  components: {},
  data() {
    return {
      isActive: true,
      edit: false,
      editNameField: "",
      iconColors: [
        "brown",
        "red",
        "orangered",
        "yellow",
        "green",
        "blue",
        "pink",
        "violet",
        "black",
      ],
      iconSizes: [
        ["S", 20],
        ["M", 30],
        ["L", 40],
      ],
      iconType: 1,
      iconSize: ["S", 20],
      iconColor: "brown",
    };
  },
  methods: {
    sendChanges() {
      this.$emit("change", {
        iconType: this.iconType,
        iconSize: this.iconSize,
        iconColor: this.iconColor,
      });
    },
    deleteMarker(index) {
      this.markers.splice(index, 1);
      localStorage.setItem("Markers", JSON.stringify(this.markers));
    },
    editName(index) {
      this.markers[index].icon.edit = !this.markers[index].icon.edit;
      this.markers[index].icon.name = this.editNameField;
      this.editNameField = "";
      localStorage.setItem("Markers", JSON.stringify(this.markers));
    },
  },
};
</script>
<style>
.selected {
  background-color: rgb(241, 106, 57) !important;
}
.icons i {
  margin: 0 10%;
}
.off {
  display: none;
}
.off2 {
  height: 6% !important;
}
div {
  cursor: pointer !important;
}
</style>
