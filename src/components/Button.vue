<template>
  <div :class="className" @click="call(this.parameter)">
    <span>{{ this.text }}</span>
  </div>
</template>

<script>
export default {
  name: "Button",
  props: {
    call: Function,
    parameter: String,
    text: String,
    className: String
  }
}
</script>