<template>
  <div>
    <button @click="addItem" class="addNewItem">Add new list item</button>
  </div>
  <ul>
    <ListElem v-for="(element, index) in elements"
    :key="index" :element="element"
    @remove="remove($event)" @modify="modify($event)"/>
  </ul>
</template>

<script>
import ListElem from './ListElem.vue'
import axios from "axios";

export default {
  data() {
    return {
      elements: []
    }
  },
  components:{
    ListElem
  },
  methods:{
    addItem(){
      let text = "Element " + (this.elements.length + 1);
      //this.elements.push({text:text, _id:this.elements.length});
      axios.post("/list", {text:text})
          .then((resp) => {
            this.elements.push({text:text, _id:resp.data.id});
          })
    },
    remove(params){
      let id = params.id;
      this.elements = this.elements.filter(
          (element) => {
            if (element._id == id) return false;
            else return  true;
          });
      axios.delete("/list", { data: {id:id} });
    },
    modify(params){
      let id = params.id;
      let value = params.value;
      this.elements = this.elements.map(
          (element) => {
            if (element._id == id) {
              element.text = value;
              return element;
            }
            else return element;
          });
      axios.put("/list", {text:value, id:id});
    },
  },
  created(){
    axios.get("/list")
        .then((response) => {
          this.elements = response.data.elements.map(
              (element)=>{
                return {_id:element._id, text: element.text}
              }
          )
        })
  }
}
</script>

<style scoped>

</style>
