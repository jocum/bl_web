<template>
<div>
  <h2>材料({{data.Width}}*{{data.Height}})利用率:{{data.Rate}}</h2>
  <canvas ref="board" class="board" />
</div>
</template>

<script>
export default {
  props: { data: Object, size: String, tkey: [String] },
  created() {
    console.log("makeuppaint:", this.w, this.data, this.size);
  },
  computed: {
    w() {
      return 914;
      // return this.data.length > 0 ? this.data[0].bgw : 100;
    },
    csize() {
      // const [w, h] = this.size.split("*");
      return [100, 200];
    },
  },
  watch: {
    data: {
      handler(val) {
        console.log("val:", val);
        if (val) {
          this.$nextTick(() => {
            this.painting(val);
          });
        }
      },
      immediate: true,
    },
  },
  methods: {
    paintingAll() {
      fetch("http://172.17.3.180:9999")
        .then(function (res) {
          return res.json();
        })
        .then(function (myjson) {
          var canvas = document.getElementById("mycanvas");
          // console.log(myjson.Boxs);
          const randColor = function () {
            return Math.round(Math.random() * 255);
          };
          myjson.Boxs.forEach((item) => {
            console.log(item);
            canvas.setAttribute("width", item.Width);
            canvas.setAttribute("height", item.Height);
            item.Rects.forEach((rect) => {
              var context = canvas.getContext("2d");
              context.strokeStyle = "black";
              context.lineWidth = 1;
              context.strokeRect(
                rect.Point.x - rect.W,
                rect.Point.y - rect.H,
                rect.W,
                rect.H
              );
              context.fillStyle = `rgba(${randColor()},${randColor()},${randColor()},0.5)`;
              context.fillRect(
                rect.Point.x - rect.W,
                rect.Point.y - rect.H,
                rect.W,
                rect.H
              );
              console.log(rect);
            });
          });

        });
    },
    painting(item = { Width: 0, Height: 0, Rects: [] }) {
      // console.log("painting:", item);
      // const canvas = document.getElementById("board");
      const canvas = this.$refs["board"];
      const randColor = function () {
        return Math.round(Math.random() * 255);
      };
      canvas.setAttribute("width", item.Width);
      canvas.setAttribute("height", item.Height);
      var context = canvas.getContext("2d");
      console.log("canvas:", canvas, context);
      item.Rects.forEach((rect) => {
        context.strokeStyle = "black";
        context.lineWidth = 1;
        context.strokeRect(
          rect.Point.x - rect.W,
          rect.Point.y - rect.H,
          rect.W,
          rect.H
        );
        context.fillStyle = `rgba(${randColor()},${randColor()},${randColor()},0.5)`;
        context.fillRect(
          rect.Point.x - rect.W,
          rect.Point.y - rect.H,
          rect.W,
          rect.H
        );
        // console.log(rect);
      });
      // if (item.UseH < item.Height){
      //   context.
      // }
    },
    pageStyle(p) {
      // const h = this.findMaxHeight(p.point)
      const { bgw, bgh } = p;
      return {
        width: `${bgw}px`,
        height: `${bgh}px`,
        transformOrigin: `top right`,
        transform: `translateX(${-bgw}px) rotate(-90deg)`,
      };
    },
    findMaxHeight(arr = []) {
      let max = 0;
      let height = 0;
      arr.forEach((e) => {
        if (e.y > max) {
          max = e.y;
          height = e.y + e.height;
        }
      });
      return height + 15;
    },
    imgStyle(d) {
      const sty = {
        width: `${d.width}px`,
        height: `${d.height}px`,
        left: `${d.x}px`,
        top: `${d.y}px`,
      };
      if (d.is_rotate) {
        sty.transformOrigin = "top left";
        sty.transform = `translateX(${d.height}px) rotate(90deg)`;
      }
      return sty;
    },
  },
};
</script>

<style  scoped>
.board {
  border: 1px solid rgb(199, 198, 198);
}
.makeup-paint {
  position: relative;
}
.page {
  border: 1px dashed red;
  position: relative;
}
.page-img {
  position: absolute;
}
</style>