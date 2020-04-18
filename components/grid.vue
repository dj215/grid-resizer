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
      :use-css-transforms="true"
    >

        <grid-item v-for="item in layoutArr"
          :x="item.x"
          :y="item.y"
          :w="item.w"
          :h="item.h"
          :i="item.i"
          :key="item.i">
            <v-btn
              @click="hadleDelete(item)"
              style="height:15px;width:15px "
              :max-width="15"
              x-small
            >
              <v-icon
                size="15"
              >
                delete
              </v-icon>
            </v-btn>
        </grid-item>
    </grid-layout>
    <nuxt-link to="/custom">Custom DnD</nuxt-link>
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
    @Prop({type: Function, required: true})
        hadleDelete!: Function
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
