<template>
  <div class="capability-type-data">

    <LabeledInput
      :formstate="formstate"
      :multiple-inputs="true"
      :name="`capability${capability.uuid}-${capability.typeData.speedOrAngle}`">

      <template #label>
        <template v-if="capability.typeData.speedOrAngle === `speed`">
          Speed / <a
            href="#anglr"
            class="button secondary inline"
            title="Specify angle instead of speed"
            @click.prevent="changeSpeedOrAngle(`angle`)">Angle</a>
        </template>
        <template v-else>
          Angle / <a
            href="#speed"
            class="button secondary inline"
            title="Specify speed instead of angle"
            @click.prevent="changeSpeedOrAngle(`speed`)">Speed</a>
        </template>
      </template>

      <EditorProportionalPropertySwitcher
        v-if="capability.typeData.speedOrAngle"
        ref="speedOrAngleInput"
        :capability="capability"
        :formstate="formstate"
        :property-name="capability.typeData.speedOrAngle" />

    </LabeledInput>

    <LabeledInput
      :formstate="formstate"
      :name="`capability${capability.uuid}-comment`"
      label="Comment">
      <PropertyInputText
        v-model="capability.typeData.comment"
        :formstate="formstate"
        :name="`capability${capability.uuid}-comment`"
        :schema-property="properties.definitions.nonEmptyString" />
    </LabeledInput>

  </div>
</template>

<script>
import schemaProperties from '../../../../lib/schema-properties.js';

import EditorProportionalPropertySwitcher from '../EditorProportionalPropertySwitcher.vue';
import LabeledInput from '../../LabeledInput.vue';
import PropertyInputText from '../../PropertyInputText.vue';

export default {
  components: {
    EditorProportionalPropertySwitcher,
    LabeledInput,
    PropertyInputText,
  },
  props: {
    capability: {
      type: Object,
      required: true,
    },
    formstate: {
      type: Object,
      required: false,
      default: null,
    },
  },
  data() {
    return {
      properties: schemaProperties,
      defaultData: {
        speedOrAngle: `speed`,
        speed: ``,
        speedStart: null,
        speedEnd: null,
        angle: ``,
        angleStart: null,
        angleEnd: null,
        comment: ``,
      },
    };
  },
  computed: {
    resetProps() {
      const resetProp = this.capability.typeData.speedOrAngle === `speed` ? `angle` : `speed`;

      return [resetProp, `${resetProp}Start`, `${resetProp}End`];
    },
  },
  methods: {
    changeSpeedOrAngle(newValue) {
      this.capability.typeData.speedOrAngle = newValue;
      this.$nextTick(() => this.$refs.speedOrAngleInput.focus());
    },
  },
};
</script>
