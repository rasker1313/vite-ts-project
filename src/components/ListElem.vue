<template>
  <li>
    <span v-if="!isMod">{{element.text}}</span>
    <input v-else type="text" :value="element.text" @blur="modify($event)"/> -
    <button @click="remove()">Del</button>
    <button @click="isMod=true">Mod</button>
  </li>
</template>

<script>
import {PropType} from "vue";

export default {
  name: "ListElem",
  data(){
    return {
      isMod: false
    }
  },
  props: ["element"],
  methods: {
    remove() {
      this.$emit("remove", {id: this.element._id});
    },
    modify(event){
      let value = event.target.value;
      this.isMod = false;
      this.$emit("modify", {id: this.element._id, value:value});
    },
    emits: ["remove", "modify"],
    updated(){
      if (this.$refs.refInput) this.$refs.refInput.focus();
    }
  }
}
</script>

<style scoped>

</style>