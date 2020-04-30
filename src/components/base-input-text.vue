<script>
export default {
  // Disable automatic attribute inheritance, so that $attrs are
  // passed to the <input>, even if it's not the root element.
  // https://vuejs.org/v2/guide/components-props.html#Disabling-Attribute-Inheritance
  inheritAttrs: false,
  // Change the v-model event name to `update` to avoid changing
  // the behavior of the native `input` event.
  // https://vuejs.org/v2/guide/components-custom-events.html#Customizing-Component-v-model
  model: {
    event: "update"
  },
  props: {
    type: {
      type: String,
      default: "text",
      // Only allow types that essentially just render text boxes.
      validator(value) {
        return [
          "email",
          "number",
          "password",
          "search",
          "tel",
          "text",
          "url"
        ].includes(value);
      }
    },
    valid: {
      type: Boolean,
      default: true
    },
    // eslint-disable-next-line vue/require-default-prop
    validator: {
      type: Object,
      required: false
      // validator($v) {
      //   return $v.hasOwnProperty('$model')
      // },
    }
  }
};
</script>

<template>
  <input
    :type="type"
    :class="[
      $style.input,
      { [$style.invalid]: (validator && validator.$error) || !valid },
    ]"
    v-bind="
      $attrs
      // https://vuejs.org/v2/guide/components-props.html#Disabling-Attribute-Inheritance
    "
    @input="$emit('update', $event.target.value)"
    @blur="$emit('ready-to-validate', $event.target.value)"
    v-on="
      $listeners
      // https://vuejs.org/v2/guide/components-custom-events.html#Binding-Native-Events-to-Components
    "
  >
</template>

<style lang="scss" module>
@import "../design/index";
.input {
  @extend %relative-block;
  @extend %config-text-input;

  @include liqid-size(100%, $grid-size * 1.25);

  overflow: hidden;
  line-height: $grid-size * 1.25;
  text-overflow: ellipsis;
  white-space: nowrap;

  &.invalid {
    border-color: $red;
  }
}
</style>
