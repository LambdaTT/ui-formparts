<template>
  <div>
    <label v-if="Label" for="editor" class="q-my-md">
      <i class="fas fa-caret-right"></i> {{ Label }}
    </label>
    <q-editor v-model="value" :disable="disable" :readonly="readonly" :placeholder="placeholder"
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
    placeholder: String,
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

<style scoped>
  h1 {
    font-size: 1.5rem;
    line-height: 15px;
  }
</style>