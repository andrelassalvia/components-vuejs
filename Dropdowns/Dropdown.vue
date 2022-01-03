<template>
  <div class="dropdown--control mb-3">
    <button class="btn--dropdown" type="button" @click.stop.prevent="toggle()">
      {{ titulo }}
      <svg
        xmlns="http://www.w3.org/2000/svg"
        class="arrow"
        fill="none"
        viewBox="0 0 24 24"
        stroke="currentColor"
      >
        <path
          stroke-linecap="round"
          stroke-linejoin="round"
          stroke-width="2"
          d="M19 9l-7 7-7-7"
        />
      </svg>
    </button>

    <div v-show="isOpen">
      <ul class="dropdown--menu">
        <slot />
      </ul>
    </div>
  </div>
</template>
<script>
import DropdownItem from "./DropdownItem.vue";
export default {
  components: { DropdownItem },

  name: "Dropdown",
  data() {
    return {
      isOpen: false,
    };
  },

  // carrega quando o componente é montado
  mounted() {
    document.addEventListener("click", this.clickOutListener);
  },

  beforeDestroy() {
    document.removeEventListener("click", this.clickOutListener);
  },

  watch: {
    // monitora toda mudanca ocorrida
    isOpen(value) {
      // 1. se is open for verdadeiro
      if (value) {
        this.$root.$emit("dropdown::open", this); // emitimos uma funcao na raiz passando todo o componente
      }
    },
  },

  created() {
    // entre em acao quando o componente e criado
    this.$root.$on("dropdown::open", this.rootCloseListener); // 2. escute a raiz, o dropdown esta aberto ()
  },

  // Utiliza com v-show para aparecer e desaparecer dropdownItem
  methods: {
    toggle() {
      this.isOpen = !this.isOpen;
    },

    close() {
      this.isOpen = false;
    },

    // fechar dropdown clicando qualquer lugar fora do componente
    clickOutListener(evt) {
      if (!this.$el.contains(evt.target)) {
        this.close();
      }
    },

    // fecha componente se clicar em outro componente
    rootCloseListener(vm) {
      if (vm != this) {
        this.close();
      }
    },
  },
  props: {
    titulo: {
      type: String,
      default: "Dropdown",
    },
  },

  // provide libera uma propriedade qualquer para todos os filhos deste componente, que sera recebido no filho usando inject
  provide() {
    return {
      dropdown: this, // estou passando o componente dropdown inteiro utilizando o this
    };
  },
};
</script>
