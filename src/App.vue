<template>
  <div id="app">
    <BackGrids :width="40" :offset="10">
      <Panel
        v-for="(p, i) in panels"
        :key="`panel-${i}`"
        :id="i"
        :offset="10"
        :width="40"
        :x="p.x"
        :y="p.y"
        :n="p.n"
        :m="p.m"
        @setPosition="setPosition"
        @setSize="setSize"
        @deletePanel="deletePanel"
      />
      <font-awesome-icon icon="plus" class="icon-add" @mousedown="addPanel"/>
    </BackGrids>
  </div>
</template>

<script>
import BackGrids from "./components/BackGrids";
import Panel from "./components/Panel";

export default {
  name: "App",
  components: {
    BackGrids,
    Panel
  },
  data: () => {
    return {
      panels: [
        {
          x: 4,
          y: 0,
          n: 4,
          m: 3
        }
      ]
    };
  },
  methods: {
    addPanel() {
      this.panels.push({
        x: 0,
        y: 0,
        n: 4,
        m: 2
      });
    },
    deletePanel(id) {
      this.panels.splice(id, 1);
    },
    setPosition({ id, x, y }) {
      this.panels[id].x = x;
      this.panels[id].y = y;
    },
    setSize({ id, n, m }) {
      this.panels[id].n = n;
      this.panels[id].m = m;
    }
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.icon-add {
  position: absolute;
  bottom: 5px;
  right: 5px;
  cursor: pointer;
}
</style>
