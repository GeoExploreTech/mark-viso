<script>
import Crosshair from "./js/crosshair.js";
import Utils from "../stuff/utils.js";

export default {
  name: "Crosshair",
  props: ["cursor", "colors", "layout", "sub", "interval", "font", "config"],
  emits: [
    "new-grid-layer",
    "redraw-grid",
    "deleteGridLayer",
    "showGridLayer",
    "layerMetaProps",
    "customEvent",
  ],
  methods: {
    create() {
      this.ch = new Crosshair(this);

      // New grid overlay-renderer descriptor.
      // Should implement draw() (see Spline.vue)
      this.$emit("new-grid-layer", {
        name: "crosshair",
        renderer: this.ch,
      });
    },
  },
  watch: {
    cursor: {
      handler: function () {
        if (!this.ch) this.create();

        // Explore = default mode on mobile
        const cursor = this.$props.cursor;
        const explore = cursor.mode === "explore";

        if (!cursor.x || !cursor.y) {
          this.ch.hide();
          this.$emit("redraw-grid");
          return;
        }
        this.ch.visible = !explore;
      },
      deep: true,
    },
  },
  render() {
    return [];
  },
};
</script>
