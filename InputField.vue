<template>
  <div :class="dense ? 'q-pa-xs' : 'q-pa-sm'">
    <!-- Input text: -->
    <q-input ref="InputFieldRef" v-if="type == 'text' || type == null" class="full-width" square filled :dense="dense"
      :clearable="clearable" :readonly="readonly" :disable="disable" v-model="value" :error="Error" @focus="() => $emit('focus')"
      type="text" :maxlength="maxlength" :label="Label" :mask="Mask" @update:model-value="updModelValue">
      <template v-slot:append>
        <slot name="buttons"></slot>
        <q-icon v-if="!!Icon" :name="Icon" color="grey-8" />
      </template>
    </q-input>

    <!-- Input password: -->
    <q-input ref="InputFieldRef" v-if="type == 'password'" class="full-width" square filled :dense="dense"
      :clearable="clearable" :readonly="readonly" :disable="disable" v-model="value" :error="Error" @focus="() => $emit('focus')"
      type="password" :maxlength="maxlength" :label="Label" @update:model-value="updModelValue">
      <template v-slot:append>
        <slot name="buttons"></slot>
        <q-icon v-if="!!Icon" :name="Icon" color="grey-8" />
      </template>
    </q-input>

    <!-- Input textarea: -->
    <q-input ref="InputFieldRef" v-if="type == 'textarea'" class="full-width" square filled :dense="dense"
      :clearable="clearable" :readonly="readonly" :disable="disable" v-model="value" :error="Error" @focus="() => $emit('focus')"
      type="textarea" :maxlength="maxlength" :label="Label" @update:model-value="updModelValue">
      <template v-slot:append>
        <slot name="buttons"></slot>
        <q-icon v-if="!!Icon" :name="Icon" color="grey-8" />
      </template>
    </q-input>

    <!-- Input email: -->
    <q-input ref="InputFieldRef" v-if="type == 'email'" class="full-width" square filled :dense="dense"
      :clearable="clearable" :readonly="readonly" :disable="disable" v-model="value" :error="Error" @focus="() => $emit('focus')"
      type="email" maxlength="255" :label="Label" @update:model-value="updModelValue">
      <template v-slot:append>
        <slot name="buttons"></slot>
        <q-icon v-if="!!Icon" :name="Icon" color="grey-8" />
      </template>
    </q-input>

    <!-- Input number: -->
    <q-input ref="InputFieldRef" v-if="type == 'number'" class="full-width" square filled :dense="dense"
      :clearable="clearable" :readonly="readonly" :disable="disable" v-model="value" :error="Error" @focus="() => $emit('focus')"
      type="number" :step="step" :max="max" :min="min" :label="Label" @update:model-value="updModelValue">
      <template v-slot:append>
        <slot name="buttons"></slot>
        <q-icon v-if="!!Icon" :name="Icon" color="grey-8" />
      </template>
    </q-input>

    <!-- Input select: -->
    <Select2 v-if="type == 'select'" :dense="dense" :clearable="clearable" :readonly="readonly" :disable="disable" :Options="Options"
      :Label="Label" :Icon="Icon" :Error="Error" @focus="() => $emit('focus')" v-model="value"
      @update:model-value="updModelValue">
    </Select2>

    <!-- Input date: -->
    <InputDate v-if="type == 'date'" :dense="dense" :readonly="readonly" v-model="value" :Default="Default"
      :Label="Label" :Error="Error" @focus="() => $emit('focus')" @update:model-value="updModelValue"></InputDate>

    <!-- Input daterange: -->
    <InputDate v-if="type == 'daterange'" :dense="dense" :readonly="readonly" range v-model="value" :Default="Default"
      :Label="Label" :Error="Error" @focus="() => $emit('focus')" @update:model-value="updModelValue"></InputDate>

    <!-- Input datetime: -->
    <InputDate v-if="type == 'datetime'" withTime :dense="dense" :readonly="readonly" v-model="value" :Default="Default"
      :Label="Label" :Error="Error" @focus="() => $emit('focus')" @update:model-value="updModelValue"></InputDate>

    <!-- Input datetimerange: -->
    <InputDate v-if="type == 'datetimerange'" range withTime :dense="dense" :readonly="readonly" v-model="value"
      :Default="Default" :Label="Label" :Error="Error" @focus="() => $emit('focus')"
      @update:model-value="updModelValue">
    </InputDate>

    <!-- Input time: -->
    <InputTime v-if="type == 'time'" :readonly="readonly" :dense="dense" :Label="Label" v-model="value"
      :Default="Default" @update:model-value="updModelValue"></InputTime>

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

    <!-- Input color: -->
    <InputColor v-if="type == 'color'" :readonly="readonly" :dense="dense" v-model="value" :Label="Label"
      :clearable="clearable" :Error="Error" :Default=Default></InputColor>
  </div>

</template>

<script>
export default {
  name: 'components-common-inputfield',

  props: {
    type: String,
    readonly: Boolean,
    disable: Boolean,
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