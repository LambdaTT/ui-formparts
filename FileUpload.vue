<template>
  <div>
    <q-input hide-bottom-space square filled :clearable="clearable" v-model="fileData.name" @click="inputClicked()"
      type="text" :label="!!Label ? Label : 'Selecione o arquivo'" @clear="clearFile"
      :color="!!Color ? Color : 'primary'" @focus="$emit('focus')" :error="Error">
      <template v-slot:prepend>
        <q-icon name="cloud_upload" />
      </template>
      <template v-slot:append>
        <q-icon v-if="!!Icon" :name="Icon" />
      </template>
    </q-input>
    <p v-show="this.fileData.size != null" class="text-caption text-right">{{ (this.fileData.size / 1024).toFixed(2) }}
      kb
    </p>
    <input id="input-file" :accept="accept" type="file" v-on:change="fileChange" style="display:none;">
  </div>
</template>
<script>
export default {
  name: 'ui-formparts-fileupload',

  props: {
    clearable: Boolean,
    accept: String,
    modelValue: Object,
    Icon: String,
    Label: String,
    Color: String,
    Error: Boolean,
    ReadAsURL: Boolean
  },

  data() {
    return {
      fileData: {
        file: null,
        name: null,
        data: null,
        size: null
      },
      dialogIsOpen: false
    }
  },

  watch: {
    modelValue: {
      handler(v) {
        this.fileData = v;
      },
      deep: true
    },

    fileData: {
      handler(v) {
        this.$emit('update:model-value', v)
      },
      deep: true
    }
  },

  methods: {
    sleep(miliseconds) {
      return new Promise((resolve) => {
        setTimeout(() => resolve(miliseconds), miliseconds);
      })
    },

    async inputClicked() {
      this.$emit('fileupload-before-choose');
      await this.sleep(100);
      this.dialogIsOpen = true;
      window.addEventListener('focus', this.onWindowFocus);
      document.getElementById('input-file').click();
      document.activeElement.blur();
    },

    clearFile() {
      var input = document.getElementById('input-file');
      input.value = null;
      this.clearFileData();
    },

    clearFileData() {
      this.fileData = {
        file: null,
        name: null,
        data: null,
        size: null
      }
    },

    readFile(file) {
      return new Promise((resolve, reject) => {
        var reader = new FileReader();
        reader.onload = function (evt) {
          resolve({
            "src": evt.target.result,
            "file": file
          })
        };

        if (this.ReadAsURL) reader.readAsDataURL(file);
        else reader.readAsText(file);
      })
    },

    async fileChange(evt) {
      this.dialogIsOpen = false;
      var input = evt.target;

      if (input.files.length > 0) {
        let filedata = await this.readFile(input.files[0]);
        this.fileData = {
          file: filedata.file,
          name: filedata.file.name,
          size: filedata.file.size,
          data: filedata.src,
        };
      } else {
        this.clearFileData();
      }

      this.$emit('fileupload-chosen')
    },

    onWindowFocus() {
      window.removeEventListener('focus', this.onWindowFocus);

      setTimeout(() => {
        if (this.dialogIsOpen)
          this.fileChange({ target: { files: [] } })
      }, 100)
    }
  }
}
</script>
