<template>
  <div>
    <div class="dropdown-container" @click="clickContainer"></div>
    <div class="dropdown" :class="{ active: dropdown.isActive }" ref="dropdown">
      <div class="dropdown-input" @click="onClick">
        <span :class="{ active: dropdown.text.length ? true : false }"
          >{{ placeholder }}
        </span>
        <small :class="{ active: dropdown.text.length ? true : false }">{{
          dropdown.text
        }}</small>
      </div>
      <input type="text" class="list-field" value="0" />
      <div class="dropdown-options" ref="list">
        <ul>
          <li
            v-for="(item, index) of dropdown.items"
            :key="index"
            @click="select(item)"
          >
            {{ item.text }}
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  emits: ["selected"],
  props: {
    items: {
      type: Array,
      required: true,
    },
    placeholder: {
      type: String,
      default: "dropdown",
    },
    sort: {
      type: Boolean,
      default: false,
    },
  },
  data() {
    return {
      dropdown: {
        height: null,
        items: null,
        text: "",
        isActive: false,
      },
    };
  },
  methods: {
    addClass(e) {
      console.log(e);
    },
    clickContainer() {
      this.dropdown.isActive = false;
      this.$refs.list.style.height = "0px";
    },
    onClick() {
      this.dropdown.isActive = !this.dropdown.isActive;
      if (this.dropdown.isActive) {
        this.$refs.list.style.height = this.dropdown.height + "px";
      } else {
        this.$refs.list.style.height = "0px";
      }
    },
    select(item) {
      this.dropdown.text = item.text;
      this.dropdown.value = item.value;
      this.dropdown.isActive = false;
      this.$refs.list.style.height = "0px";
      this.$emit("selected", item.value);
    },
  },
  mounted() {
    if (this.sort) {
      let map = this.items.map(function (el, i) {
        return { i, text: el.text.toLowerCase(), value: el.value };
      });
      map.sort(function (a, b) {
        if (a.text > b.text) {
          return 1;
        }
        if (a.text < b.text) {
          return -1;
        }
        return 0;
      });
      let result = map.map((el) => {
        return this.items[el.i];
      });
      this.dropdown.items = result;
    } else {
      this.dropdown.items = this.items;
    }

    this.$nextTick(() => {
      const list = this.$refs.list;
      let listHeight = list.clientHeight;
      this.dropdown.height = listHeight;
      list.style.height = "0px";
    });
  },
};
</script>

<style lang="css" scoped>
@import url("https://fonts.googleapis.com/css2?family=Poppins:wght@100;200;300;400;500;600;700;800;900&display=swap");

.dropdown {
  position: relative;
  font-family: "Poppins", sans-serif;
  box-sizing: border-box;
  max-width: 300px;
  width: 100%;
  z-index: 2;
}
.dropdown-container {
  position: absolute;
  width: 100%;
  height: 100%;
}
.dropdown-input {
  padding: 10px 35px 10px 20px;
  cursor: pointer;
  height: 50px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  position: relative;
  border-bottom: 1px solid #494d59;
}
.dropdown-input::before {
  content: "";
  display: block;
  width: 15px;
  height: 15px;
  background-image: url("../assets/down-arrow.png");
  background-repeat: no-repeat;
  background-size: cover;
  background-position: center;
  position: absolute;
  right: 10px;
  top: 50%;
  transition: all 0.3s ease-in-out;
  transform: translateY(-50%);
}
.dropdown-input::after {
  content: "";
  position: absolute;
  display: block;
  left: 50%;
  transform: translateX(-50%);
  bottom: 0;
  width: 0;
  height: 1px;
  background-color: #494d59;
  transition: all 0.3s ease-in-out;
}
.dropdown.active .dropdown-input::after {
  width: 100%;
  background-color: blue;
}
.dropdown-input span {
  font-size: 20px;
  display: block;
  transition: all 0.5s ease-in-out;
  opacity: 0.5;
}
.dropdown-input span.active {
  font-size: 15px !important;
  transform: translateY(-10px) !important;
}
.dropdown-input small {
  font-size: 20px;
  position: absolute;
  bottom: 5px;
  transform: translateY(10px);
  opacity: 0;
  transition: all 0.5s ease-in-out;
}
.dropdown-input small.active {
  transform: translateY(0);
  opacity: 1;
}

.dropdown input {
  display: none;
}
.dropdown-options {
  overflow: hidden;
  transition: all 0.3s ease-in-out;
  box-shadow: 0 0 2px #494d59;
}
.dropdown-options ul {
  margin: 0;
  padding: 10px 0;
}
.dropdown-options ul li {
  cursor: pointer;
  padding-left: 10px;
  font-size: 20px;
}
.dropdown-options ul li:hover {
  background-color: #a2a7b6;
  color: #fff;
}
.dropdown-options ul li:active {
  background-color: #494d59;
}

.dropdown.active span {
  font-size: 15px;
  color: blue;
}

.dropdown.active .dropdown-input::before {
  transform: rotateX(-180deg);
  transition: all 0.3s ease-in-out;
}
</style>