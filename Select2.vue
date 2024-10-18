<template>
  <q-select hide-bottom-space :bg-color="`bg-${BgColor ? BgColor : 'white'}`" option-disable="inactive" :disable="disable" :hide-dropdown-icon="readonly" :dense="dense" filled square
    :clearable="clearable" v-model="selected" use-input hide-selected fill-input input-debounce="300" :options="options"
    @filter="filterFn" :label="Label" :error="Error" @focus="$emit('focus')" :readonly="readonly">
    <template v-if="!!Icon" v-slot:append>
      <q-icon :name="Icon" color="grey-8" />
    </template>
    <template v-slot:no-option>
      <q-item>
        <q-item-section class="text-grey">
          Nenhum resultado
        </q-item-section>
      </q-item>
    </template>
  </q-select>
</template>

<script>
export default {
  name: 'datatable-component',

  props: {
    BgCOlor: String,
    Options: Array,
    Label: String,
    Icon: String,
    Error: Boolean,
    readonly: Boolean,
    disable: Boolean,
    dense: Boolean,
    modelValue: {
      types: ['string', 'object']
    },
    clearable: Boolean
  },

  data() {
    return {
      options: [],
      selected: null,
    }
  },

  watch: {
    selected() {
      var selectedValue = !!this.selected ? this.selected.value : null;

      this.$emit('update:model-value', selectedValue);
    },

    modelValue() {
      this.setValue()
    }
  },

  methods: {
    setValue() {
      if (!!this.modelValue == false) this.selected = null;
      else {
        this.selected = this.Options.filter((v) =>
          JSON.stringify(v.value).toLocaleLowerCase() == JSON.stringify(this.modelValue).toLocaleLowerCase()
        )[0];
      }
    },

    filterFn(val, update) {
      if (!!val == false || val === '') {
        update(() => {
          this.options = this.Options;
        })
        return
      }

      update(() => {
        const needle = val.toLowerCase()
        this.options = this.Options.filter(v => v.label.toLowerCase().includes(needle));
      })
    },
  },

  created() {
    setTimeout(() => {
      this.setValue();
    }, 200);
  }
}
</script>