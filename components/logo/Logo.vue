<template>
  <svg
    class="NuxtLogo transform"
    :class="{ 'scale-125': scaling === 25, 'scale-110': scaling === 10 }"
    width="32"
    height="32"
    xmlns="http://www.w3.org/2000/svg"
  >
    <g>
      <rect
        id="canvas_background"
        fill="none"
        height="402"
        width="582"
        y="-1"
        x="-1"
      />
    </g>
    <g>
      <ellipse
        id="svg_1"
        :fill="color"
        ry="5.94"
        rx="4"
        cy="15.931974"
        cx="16.068026"
      />
      <path
        id="svg_2"
        :fill="color"
        d="m30.65,13.24l-14,-12a1,1 0 0 0 -1.3,0l-14,12a1,1 0 1 0 1.3,1.52l0.68,-0.59l0,13.83a3,3 0 0 0 3,3l19.34,0a3,3 0 0 0 3,-3l0,-13.83l0.68,0.59a1,1 0 1 0 1.3,-1.52zm-13.65,10.42l0,2.34a1,1 0 0 1 -2,0l0,-2.34a8.38,8.38 0 0 1 0.69,-15.61a0.94,0.94 0 0 1 0.62,0a8.38,8.38 0 0 1 0.69,15.61z"
      />
    </g>
  </svg>
</template>
<script>
export default {
  props: {
    scale: {
      type: Number,
      default: 0,
    },
    color: {
      type: String,
      default: '#1f2937',
    },
  },
  computed: {
    scaling() {
      return this.scale
    },
  },
}
</script>
<style scoped>
.NuxtLogo {
  animation: 0.2s appear;
}

@keyframes appear {
  0% {
    opacity: 0;
  }
}
</style>
