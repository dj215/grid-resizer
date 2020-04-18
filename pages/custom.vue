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
        <div class="column-wrapper" id="columns">
            <div class="box">
                <div v-for="column in columnArr" :id="column" :key="column" class="column" draggable="true"><p>column{{column}}</p></div>
            </div>
        </div>
    </div>
</template>

<script lang="ts">
    import Vue, { PropOptions } from 'vue'
    var dragSrcEl:any = null;
    export default Vue.extend({
        name: "Draggable",
        data(){
            return{
                column_no:new Number(),
                columnArr:new Array<number>(),
                styleObj:new Object()
            }
        },
        updated(){
            var dropTarget = document.querySelector('.column-wrapper');
            dropTarget.addEventListener('drop',(e :any)=> this.handleDrop(e), false);
            var cols = document.querySelectorAll('.column');
                cols.forEach(col => {
                    col.addEventListener('dragstart',(e:any) =>this.handleDragStart(e), false);
                })
        },
        mounted(){
            var dropTarget = document.querySelector('.column-wrapper');
            dropTarget.addEventListener('dragover',(e :any) => this.handleDragOver(e), false);
            dropTarget.addEventListener('drop',(e :any)=> this.handleDrop(e), false);
            var cols = document.querySelectorAll('.column');
            cols.forEach(col => {
                col.addEventListener('dragstart',(e:any) =>this.handleDragStart(e), false);
            })
        },
        methods:{
            handleAdd(){
                for(let i=0;i<this.column_no;i++){ 
                    this.columnArr.push(i);
                } 
            },
            handleDragEnd(e:any) {
                var cols = document.querySelectorAll('#columns .column');
                cols.forEach(col => {
                    col.classList.remove('over')
                })
                let droppable  = e.target.classList.contains('box');
                let srcId = e.dataTransfer.getData("srcId");
                if (droppable) {
                    e.target.appendChild(document.getElementById(srcId));
                }
                // if (dragSrcEl != e) {
                //     dragSrcEl.innerHTML = e.target.innerHTML;
                //     e.target.innerHTML = e.dataTransfer.getData('text/html');
                // }
                // e.target.style.opacity = '1'; 
            },
            handleDrop(e:any) {

                if (e.stopPropagation) {
                    e.stopPropagation(); // stops the browser from redirecting.
                }
                console.log(e.target);
                
                let droppable  = e.target.classList.contains('box');
                let srcId = e.dataTransfer.getData("srcId");
                console.log(srcId,droppable);
                
                if (droppable) {
                    console.log("append");
                    
                    e.target.appendChild(document.getElementById(srcId));
                }
                // if (dragSrcEl != e.target) {

                //     dragSrcEl.innerHTML = e.target.innerHTML;
                //     e.target.innerHTML = e.dataTransfer.getData('text/html');
                // }
                return false;
            },
            handleDragStart(e:any) {
                // e.target.style.opacity = '0.4'; 
                // dragSrcEl = e.target;
                e.dataTransfer.effectAllowed = 'move';
                e.dataTransfer.setData('srcId', e.target.id);
                // const offsetParent = e.target.offsetParent || document.body;
                // const offsetParentRect = e.offsetParent === document.body ? {left: 0, top: 0} : offsetParent.getBoundingClientRect();

                // const x = e.clientX + offsetParent.scrollLeft - offsetParentRect.left;
                // const y = e.clientY + offsetParent.scrollTop - offsetParentRect.top;
            },
            handleDragOver(e:any) {
                if (e.preventDefault) {
                    e.preventDefault(); 
                }
                e.dataTransfer.dropEffect = 'move'; 
                return false;
            },
            handleDragEnter(e:any) {    
                e.target.classList.add('over');
            },
            handleDragLeave(e:any) {
                e.target.classList.remove('over'); 
            }

        },

    })
</script>
<style scoped>
[draggable] {
  user-select: none;
}
.column.over {
  background-color: pink;
}
.resizable{
    resize: both;
}
.box{
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(100px, 1fr));
}
.column {
  height: 150px;
  /* width: 150px;  */
  float: left;
  border: 2px solid #666666;
  background-color: #ccc;
  margin-right: 5px;
  border-radius: 10px;
  box-shadow: inset 0 0 3px #000;
  text-align: center;
  cursor: move;
  resize: both;
  margin: 10px;
  overflow: auto;
  
}
.column header {
  color: #fff;
  text-shadow: #000 0 1px;
  box-shadow: 5px;
  padding: 5px;
  background: -moz-linear-gradient(left center, rgb(0,0,0), rgb(79,79,79), rgb(21,21,21));
  background: -webkit-linear-gradient(left center, rgb(0,0,0), rgb(79,79,79), rgb(21,21,21));
  background: -ms-linear-gradient(left center, rgb(0,0,0), rgb(79,79,79), rgb(21,21,21));
  border-bottom: 1px solid #ddd;
  border-top-left-radius: 10px;
  border-top-right-radius: 10px;
}
</style>
