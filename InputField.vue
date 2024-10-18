<template>
  <div class="q-pa-sm">
    <!-- Input text: -->
    <q-input hide-bottom-space v-if="type == 'text' || type == null"
      :class="`full-width bg-${BgColor ? BgColor : 'white'}`" square filled :dense="dense" :clearable="clearable"
      :readonly="readonly" v-model="value" :error="Error" @focus="() => $emit('focus')" type="text"
      :maxlength="maxlength" :label="Label" :mask="Mask" @update:model-value="updModelValue">
      <template v-slot:append>
        <q-icon v-if="!!Icon" :name="Icon" color="grey-8" />
      </template>
    </q-input>

    <!-- Input password: -->
    <q-input hide-bottom-space v-if="type == 'password'" :class="`full-width bg-${BgColor ? BgColor : 'white'}`" square
      filled :dense="dense" :clearable="clearable" :readonly="readonly" v-model="value" :error="Error"
      @focus="() => $emit('focus')" type="password" :maxlength="maxlength" :label="Label"
      @update:model-value="updModelValue">
      <template v-slot:append>
        <q-icon v-if="!!Icon" :name="Icon" color="grey-8" />
      </template>
    </q-input>

    <!-- Input textarea: -->
    <q-input hide-bottom-space v-if="type == 'textarea'" :class="`full-width bg-${BgColor ? BgColor : 'white'}`" square
      filled :dense="dense" :clearable="clearable" :readonly="readonly" v-model="value" :error="Error"
      @focus="() => $emit('focus')" type="textarea" :maxlength="maxlength" :label="Label"
      @update:model-value="updModelValue">
      <template v-slot:append>
        <q-icon v-if="!!Icon" :name="Icon" color="grey-8" />
      </template>
    </q-input>

    <!-- Input email: -->
    <q-input hide-bottom-space v-if="type == 'email'" :class="`full-width bg-${BgColor ? BgColor : 'white'}`" square
      filled :dense="dense" :clearable="clearable" :readonly="readonly" v-model="value" :error="Error"
      @focus="() => $emit('focus')" type="email" maxlength="255" :label="Label" @update:model-value="updModelValue">
      <template v-slot:append>
        <q-icon v-if="!!Icon" :name="Icon" color="grey-8" />
      </template>
    </q-input>

    <!-- Input number: -->
    <q-input hide-bottom-space v-if="type == 'number'" :class="`full-width bg-${BgColor ? BgColor : 'white'}`" square
      filled :dense="dense" :clearable="clearable" :readonly="readonly" v-model="value" :error="Error"
      @focus="() => $emit('focus')" type="number" :step="step" :max="max" :min="min" :label="Label"
      @update:model-value="updModelValue">
      <template v-slot:append>
        <q-icon v-if="!!Icon" :name="Icon" color="grey-8" />
      </template>
    </q-input>

    <!-- Input select: -->
    <Select2 :BgColor="BgColor" v-if="type == 'select'" :dense="dense" :clearable="clearable" :readonly="readonly"
      :Options="Options" :Label="Label" :Icon="Icon" :Error="Error" @focus="() => $emit('focus')" v-model="value"
      @update:model-value="updModelValue">
    </Select2>

    <!-- Input date: -->
    <InputDate :BgColor="BgColor" v-if="type == 'date'" :dense="dense" :readonly="readonly" v-model="value"
      :Default="Default" :Label="Label" :Error="Error" @focus="() => $emit('focus')"
      @update:model-value="updModelValue">
    </InputDate>

    <!-- Input daterange: -->
    <InputDate :BgColor="BgColor" v-if="type == 'daterange'" :dense="dense" :readonly="readonly" range v-model="value"
      :Default="Default" :Label="Label" :Error="Error" @focus="() => $emit('focus')"
      @update:model-value="updModelValue">
    </InputDate>

    <!-- Input datetime: -->
    <InputDate :BgColor="BgColor" v-if="type == 'datetime'" withTime :dense="dense" :readonly="readonly" v-model="value"
      :Default="Default" :Label="Label" :Error="Error" @focus="() => $emit('focus')"
      @update:model-value="updModelValue">
    </InputDate>

    <!-- Input datetimerange: -->
    <InputDate :BgColor="BgColor" v-if="type == 'datetimerange'" range withTime :dense="dense" :readonly="readonly"
      v-model="value" :Default="Default" :Label="Label" :Error="Error" @focus="() => $emit('focus')"
      @update:model-value="updModelValue">
    </InputDate>

    <!-- Input time: -->
    <InputTime :BgColor="BgColor" v-if="type == 'time'" :readonly="readonly" :dense="dense" :Label="Label"
      v-model="value" :Default="Default" @update:model-value="updModelValue"></InputTime>

    <!-- Input color: -->
    <InputColor :BgColor="BgColor" v-if="type == 'color'" :readonly="readonly" :dense="dense" v-model="value"
      :Label="Label" :clearable="clearable" :Error="Error" :Default=Default></InputColor>

    <!-- Input file: -->
    <q-file v-if="type == 'file'" :clearable="clearable" :accept="accept" filled square bottom-slots v-model="value"
      :label="Label" counter @update:model-value="updModelValue">
      <template v-slot:prepend>
        <q-icon name="cloud_upload" />
      </template>
      <template v-slot:append>
        <q-icon v-if="!!Icon" :name="Icon" />
      </template>
    </q-file>
  </div>

</template>

<script>
export default {
  name: 'components-common-inputfield',

  props: {
    BgColor: String,
    type: String,
    readonly: Boolean,
    dense: Boolean,
    modelValue: [String, Object],
    Icon: String,
    Label: String,
    Mask: String,
    clearable: Boolean,
    Options: Array,
    Error: Boolean,
    maxlength: String,
    step: String,
    max: String,
    min: String,
    Default: [String, Object],
    accept: String,
  },

  data() {
    return {
      value: null
    }
  },

  watch: {
    modelValue(v) {
      this.value = v;
    }
  },

  methods: {
    updModelValue(v) {
      this.$emit('update:model-value', v);
    }
  },

  created() {
    this.value = this.modelValue;
  },

  mounted() {
    this.$emit('expose-ref', this.$refs.InputFieldRef);
  }
}
</script>