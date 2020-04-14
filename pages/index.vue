<template>
  <div>
    <v-text-field
      label="No. of Columns"
      v-model="column_no"
      style="width:250px;"
      type="number"
    >
    </v-text-field>
    <v-btn
      :disabled="!column_no"
      @click="handleAdd"
    >Add</v-btn>
    <grid
      :layout="layout"
    ></grid>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'nuxt-property-decorator'
import Grid from '@/components/grid.vue'

@Component({
  components:{
    Grid
  }
})
export default class Index extends Vue {
  column_no:any = null;
  layout:Array<object> = []
   public handleAdd(): void{
      //caculate no.of grid per row
      let grid = Math.ceil(12/this.column_no);

      for(let i=0;i<this.column_no;i++){     
        let newColumn = {
          x:(Number(this.layout.length) % (12/grid))*grid,
          y:0,
          w:grid,
          h:grid,
          i:this.layout.length
        }
        this.layout.push(newColumn)
      }
      this.column_no = null
  }
}
</script>
