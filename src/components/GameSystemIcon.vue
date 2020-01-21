<template>
  <div
    class="system-icon"
    v-html="getSimpleIcon(this.type) + this.type"
    :style="iconVars"/>
</template>

<script>
import SimpleIcons from 'simple-icons'

export default {
  name: 'game-table',
  props: {
    type: String,
    size: Number,
  },
  methods: {
    getSimpleIcon(system){

      switch(system) {
        case "Xbox":
          return SimpleIcons.get('xbox').svg;
        case "PS4":
          return SimpleIcons.get('playstation').svg;
        case "Switch":
          return SimpleIcons.get('nintendoswitch').svg;
        default:
          return "No Icon"
      }
    },
  },
  computed: {
    iconVars() {
      return {
        '--height': this.size + 'px',
        '--width' : this.size + 'px'
      }
    }
  },
}

</script>

<style lang="scss">
.system-icon {
  display: flex;
  align-items: center;
  svg{
    fill: var(--font-color);
    height: var(--height);
    width: var(--width);
    margin-right: .5rem;
  }
}

</style>
