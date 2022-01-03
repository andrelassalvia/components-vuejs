<template>
  <Component
    :is="is"
    class="dropdown--item"
    :href="href"
    :to="to"
    @click="onClick"
  >
    <slot />
  </Component>
</template>
<script>
export default {
  name: "DropdownItem",

  inject: ["dropdown"],

  props: {
    stateName: {
      type: String,
    },
    sigla: {
      type: String,
    },

    href: {
      type: String,
      default: undefined,
    },

    to: {
      type: [String, Object],
      default: undefined,
    },
  },

  computed: {
    is() {
      if (typeof this.to !== "undefined") {
        return "RouterLink";
      }
      if (typeof this.href !== "undefined") {
        return "a";
      } else {
        return "button";
      }
    },
  },

  methods: {
    onClick(evt) {
      this.$emit("click", evt);
      this.closeDropdown();
    },

    // fechar o dropdown
    closeDropdown() {
      this.dropdown.close();
    },
  },
};
</script>
