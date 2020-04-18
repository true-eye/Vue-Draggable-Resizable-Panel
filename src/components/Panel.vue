<template>
  <div>
    <div
      class="panel ghost"
      v-if="type == 1"
      :style="{
      width: (mx - dx - (x * width + (2 * x + 1) * offset)) + 'px',
      height: (my - dy - (y * width + offset * (2 * y + 1))) + 'px',
      left: (x * width + (2 * x + 1) * offset) + 'px',
      top: (y * width + offset * (2 * y + 1)) + 'px',
     }"
    >
      <font-awesome-icon icon="arrows-alt" class="icon-move"/>
      <font-awesome-icon icon="trash-alt" class="icon-trash"/>
      <font-awesome-icon icon="expand-alt" class="icon-expand"/>
    </div>
    <div
      class="panel"
      :style="{
        width: (n * width + 2 * offset * (n-1)) + 'px',
        height: (m * width + 2 * offset * (m-1)) + 'px',
        left: (x * width + (2 * x + 1) * offset) + 'px',
        top: (y * width + offset * (2 * y + 1)) + 'px',
        backgroundColor: type == 1 ? 'rgba(230, 230, 230, 0.3)' : undefined
      }"
      v-on:dragstart="dragStart"
      v-on:drag="dragging"
      @dragend="dragEnd"
      draggable="true"
    >
      <input type="text" :value="msg" @change="updateText" placeholder="Write Something...">
      <font-awesome-icon
        v-if="type !== 1"
        icon="arrows-alt"
        class="icon-move"
        @mousedown="setType(0)"
      />
      <font-awesome-icon
        v-if="type !== 1"
        icon="trash-alt"
        class="icon-trash"
        @click="deletePanel"
      />
      <font-awesome-icon
        v-if="type !== 1"
        icon="expand-alt"
        class="icon-expand"
        @mousedown="setType(1)"
      />
    </div>
  </div>
</template>

<script>
export default {
  name: "Panel",
  props: {
    id: Number,
    n: Number,
    m: Number,
    width: Number,
    offset: Number,
    x: Number,
    y: Number
  },
  data: () => ({
    dx: 0,
    dy: 0,
    mx: 0,
    my: 0,
    type: -1,
    msg: ""
  }),
  methods: {
    updateText: function(event) {
      this.msg = event.target.value;
    },
    deletePanel: function(type) {
      this.$emit("deletePanel", this.id);
    },
    setType: function(type) {
      this.type = type;
    },
    move: function(event) {
      if (!event.clientX || !event.clientY) return;
      const tx = event.clientX - this.dx;
      const ty = event.clientY - this.dy;

      const newX = Math.floor(
        (tx + this.offset) / (2 * this.offset + this.width)
      );
      const newY = Math.floor(
        (ty + this.offset) / (2 * this.offset + this.width)
      );
      if (this.x !== newX || this.y !== newY)
        this.$emit("setPosition", { id: this.id, x: newX, y: newY });
    },
    resize: function(event) {
      if (!event.clientX || !event.clientY) return;

      this.mx = event.clientX;
      this.my = event.clientY;

      const bx = event.clientX - this.dx;
      const by = event.clientY - this.dy;

      const newX = Math.floor(
        (bx - this.offset) / (2 * this.offset + this.width)
      );
      const newY = Math.floor(
        (by - this.offset) / (2 * this.offset + this.width)
      );

      const newN = newX - this.x + 1;
      const newM = newY - this.y + 1;
      // console.log(newX, newY, newN, newM);

      if (this.n !== newN || this.m !== newM)
        this.$emit("setSize", { id: this.id, n: newN, m: newM });
    },
    dragStart: function(event) {
      var img = document.createElement("img");
      img.src =
        "data:image/gif;base64,R0lGODlhAQABAIAAAAUEBAAAACwAAAAAAQABAAACAkQBADs=";
      if (this.type == 1) event.dataTransfer.setDragImage(img, 0, 0);

      const mx = event.clientX;
      const my = event.clientY;

      this.mx = mx;
      this.my = my;

      let tx = this.x * this.width + (2 * this.x + 1) * this.offset;
      let ty = this.y * this.width + (2 * this.y + 1) * this.offset;

      if (this.type == 1) {
        tx += this.n * this.width + 2 * this.offset * (this.n - 1);
        ty += this.m * this.width + 2 * this.offset * (this.m - 1);
      }

      this.dx = mx - tx;
      this.dy = my - ty;
    },
    dragging: function(event) {
      if (this.type == 0) this.move(event);
      else if (this.type == 1) this.resize(event);
    },
    dragEnd: function(event) {
      if (this.type == 0) this.move(event);
      this.type = -1;
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.panel {
  position: absolute;
  background-color: rgba(230, 230, 230, 0.1);
  border: 1px dotted red;
  z-index: 4;
}
.panel input {
  border: none;
  background: transparent;
  width: fit-content;
  outline: none;
  position: absolute;
  left: 15px;
  top: 20px;
}
.ghost {
  z-index: 5;
  pointer-events: none;
  background-color: rgba(230, 230, 230, 0.7);
}
.panel:hover {
  background-color: rgba(230, 230, 230, 0.7);
}
.panel svg {
  position: absolute;
  color: transparent;
  z-index: 3;
  cursor: pointer;
}
.panel:hover svg {
  color: black;
}
.ghost svg {
  color: black !important;
}
.panel .icon-move {
  left: 5px;
  top: 5px;
}
.panel .icon-trash {
  right: 5px;
  top: 5px;
}
.panel .icon-expand {
  right: 5px;
  bottom: 5px;
  transform: rotateZ(90deg);
}
</style>
