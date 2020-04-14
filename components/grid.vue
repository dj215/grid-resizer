<template>
  <div>
    <grid-layout
      :layout="layoutArr"
      :col-num="12"
      :row-height="30"
      :is-draggable="true"
      :is-resizable="true"
      :is-mirrored="false"
      :vertical-compact="true"
      :margin="[10, 10]"
      :autoSize="true"
      :responsive="true"
      :use-css-transforms="true"
    >

        <grid-item v-for="item in layoutArr"
          :x="item.x"
          :y="item.y"
          :w="item.w"
          :h="item.h"
          :i="item.i"
          :key="item.i">
        </grid-item>
    </grid-layout>
  </div>
</template>

<script lang="ts">
import { Component, Vue, Prop } from 'nuxt-property-decorator'
import VueGridLayout from 'vue-grid-layout';
@Component({
  components:{
    GridLayout: VueGridLayout.GridLayout,
    GridItem: VueGridLayout.GridItem
  }
})
export default class Grid extends Vue {
    @Prop({ type: Array, required: true })
        layout!: object[];
    
    get layoutArr(): Array<object> {
        return this.layout
    }
}
</script>
<style scoped>
    .vue-grid-item:not(.vue-grid-placeholder){
      background: #ccc;
      border: 1px solid black;
    }
</style>
