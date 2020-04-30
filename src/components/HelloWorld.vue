<script>
import BaseInputText from "./base-input-text";
import { required, requiredIf } from "vuelidate/lib/validators";
export default {
  components: {
    BaseInputText
  },
  data() {
    return {
      serverConfigEdit: {
        server: null,
        fabric_id: null,
        group_create: true,
        group_info: ""
      }
    };
  },
  computed: {
    groupCreate: function() {
      return this.serverConfigEdit.group_create;
    }
  },
  validations: {
    serverConfigEdit: {
      server: {
        required
      },
      fabric_id: {
        required
      },
      group_create: {
        required
      },
      group_info: {
        required: requiredIf(function() {
          return !this.serverConfigEdit.group_create;
        })
      }
    }
  }
};
</script>

<template>
  <div :class="$style.container">
    <div :class="$style.mainColumn">
      <div :class="$style.header">
        <h1>Slurm</h1>
      </div>
      <div :class="$style.form">
        <label for="fabricId" :class="$style.textLabel">Fabric ID</label>
        <BaseInputText id="fabricId" v-model="serverConfigEdit.fabric_id"></BaseInputText>
        <label :class="$style.textLabel" for="server">Liqid Server</label>
        <BaseInputText id="server" v-model="serverConfigEdit.server"></BaseInputText>

        <p :class="$style.groupInstructions">Designate group to be used by Slurm cluster:</p>
        <div :class="$style.radioWrapper">
          <input id="autoCreate" v-model="serverConfigEdit.group_create" type="radio" value="true">
          <label
            for="autoCreate"
            :class="[$style.radioLabel, {[$style.highlighted]: groupCreate}]"
          >Automatically create group</label>
        </div>
        <div :class="[$style.radioWrapper]">
          <input
            id="customGroup"
            v-model="serverConfigEdit.group_create"
            type="radio"
            value="false"
          >
          <label
            for="customGroup"
            :class="[$style.radioLabel, {[$style.highlighted]: !groupCreate}]"
          >Use existing group:</label>
          <span :class="$style.groupInfoInput">
            <BaseInputText
              id="groupInfo"
              v-model="serverConfigEdit.group_info"
              :disabled="groupCreate"
              placeholder="Enter group name"
            ></BaseInputText>
          </span>
        </div>
      </div>
    </div>
  </div>
</template>

<style lang="scss" module>
@import "../design/index";
.container {
  @include liqid-size(100%, calc(100% - 108px));

  box-sizing: border-box;
  display: flex;
  justify-content: center;
  font-size: 2em;
  color: $light-grey;

  h1 {
    @extend %heading;
  }
  p {
    margin-top: $grid-size;
    line-height: $grid-size;
  }
}
.mainColumn {
  @include liqid-size($column-full-width, 100%);

  padding-top: $grid-size;
}
.header {
  display: flex;
  align-items: center;
  h1 {
    flex: 1;
  }
}
.textLabel {
  @extend %body-copy;
  @extend %relative-block;

  width: 100%;
  height: $grid-size * 1.5;
  margin-top: $grid-size;
  line-height: $grid-size;
  color: $light-grey;
}
.groupInstructions {
  padding-top: 20px;
}
.radioWrapper {
  display: flex;
  input[type="radio"] {
    position: absolute;
    opacity: 0;
  }

  .radioLabel::before {
    display: inline-block;
    width: 16px;
    height: 16px;
    margin-right: $grid-size / 2;
    content: "";
    background-color: darken($color: $light-grey, $amount: 25);
    border-radius: 50%;
    box-shadow: inset 0 0 3px -1px darken($color: $grey, $amount: 40);
  }

  input[type="radio"]:focus-within + .radioLabel::before {
    box-shadow: 0 0 0 3px darken($color: $blue, $amount: 22);
  }

  input[type="radio"]:checked + .radioLabel::before {
    background-image: radial-gradient(circle at center, $grey 23%, $blue 28%);
  }
}
.radioLabel {
  @extend %body-copy;

  display: flex;
  align-items: center;
  width: auto;
  height: $grid-size * 1.5;
  white-space: nowrap;
  &.highlighted {
    @extend %body-copy-highlight;
  }
}
.groupInfoInput {
  flex: 1;
  height: $grid-size * 1.5;
  padding-top: 3px;
  margin-left: $grid-size / 2;
}
</style>