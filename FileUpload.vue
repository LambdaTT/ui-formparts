<template>
  <div>
    <q-input square filled :clearable="clearable" v-model="fileData.name" @click="inputClicked()" type="text"
      label="Selecione o arquivo" @clear="clearFile" color="primary">
      <template v-slot:prepend>
        <q-icon name="cloud_upload" />
      </template>
    </q-input>
    <p v-show="this.fileData.size != null" class="text-caption text-right">{{ (this.fileData.size / 1024).toFixed(2) }}
      kb
    </p>
    <input id="input-file" type="file" v-on:change="fileChange" :accept="!!accept ? accept : '*/*'" style="display:none;">
  </div>
</template>
<script>
import Utils from '../../../services/utils';

export default {
  name: 'component-fileupload',

  props:{
    clearable: Boolean,
    accept: String
  },

  data() {
    return {
      fileData: {
        file: null,
        name: null,
        data: null,
        size: null
      }
    }
  },

  methods: {
    inputClicked() {
      document.getElementById('input-file').click();
      document.activeElement.blur();
    },

    clearFile() {
      var input = document.getElementById('input-file');
      input.value = null;
      this.clearFileData();
    },

    clearFileData() {
      for (let k in this.fileData)
        this.fileData[k] = null;

      this.$emit('file-change', this.fileData);
    },

    fileChange(evt) {
      var input = evt.target;
      if (input.files.length > 0) {
        Utils.readFile(input.files[0], (filedata) => {
          this.fileData.file = filedata.file;
          this.fileData.name = filedata.file.name;
          this.fileData.size = filedata.file.size;
          this.fileData.data = filedata.src;

          this.$emit('file-change', this.fileData);
        }, true);
      } else {
        this.clearFileData();
      }
    }
  },

  created() {
    this.$emit('set-clear-input', this.clearFile);
  }
}
</script>
