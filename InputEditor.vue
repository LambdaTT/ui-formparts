<template>
  <div>
    <q-editor v-model="value" :label="Label" :disable="disable" :readonly="readonly"
      :class="`full-width ${this.Error ? 'error' : ''}`"
      @update:model-value="$emit('focus')">
    </q-editor>
  </div>
</template>

<script>
import { readonly } from 'vue';


export default{
  name: 'ui-formparts-inputeditor',

  props:{
    modelValue: String,
    disable: Boolean,
    readonly: Boolean,
    Label: String,
    Error: Boolean,
  },

  data(){
    return {
      value: "",
      
    }
  },

  watch:{
    // Input
    modelValue(v){
      if(v === null) {
        this.value = '';
      } else {
        this.value = v;
      }
    },

    // Output
    value(v){
      if(v == '<br>') {
        v = '';
      }
      this.$emit('update:model-value', (v == '') ? null : v);
    }
  },
}
</script>

<style scoped>
  .error{
    border: 2px solid red;
  }
</style>