<template>
  <section>
    <label>{{ label }}</label>
    <div v-if="multiple" class="select" @click.prevent="handleSelect(id)" :id="id">
      <div v-for="value in values" :key="value" :id="value" class="multiselected" @click.prevent="$emit('remove')">
        {{ value }}
        <IconClose />
      </div>
      <IconUp v-if="isHidden === id" />
      <IconDown v-else />
    </div>
    <div v-else class="select" @click.prevent="handleSelect(id)" :id="id">
      <div class="selected">{{ value }}</div>
      <IconUp v-if="isHidden === id" />
      <IconDown v-else />
    </div>
    <div v-if="isHidden === id" class="options">
      <button v-for="option in options" :key="option" :id="option" @click.prevent="handleSelected">{{ option }}</button>
    </div>
    <section v-for="error in errors" :key="error.id" class="form__error">
      <div v-if="error.show">{{ error.message }}</div>
    </section>
  </section>
</template>

<script>
  import IconClose from './icons/IconClose.vue';
  import IconUp from './icons/IconUp.vue';
  import IconDown from './icons/IconDown.vue';

  export default {
    props: {
      id: String,
      label: String,
      options: Array,
      value: String,
      values: Array,
      multiple: Boolean,
      errors: Array,
    },
    components: { IconClose, IconUp, IconDown },
    data() {
      return {
        isHidden: null,
      };
    },
    mounted() {
      document.addEventListener('click', this.handleClick);
    },
    methods: {
      handleSelect(key) {
        this.isHidden ? this.isHidden = null : this.isHidden = key;
      },
      handleSelected(e) {
        this.$emit('setvalue', this.id, e.target.id);
        this.isHidden = null;
      },
      handleClick(e) {
        if (e.target.id !== this.id) {
          this.isHidden = null;
        }
      }
    },
  };
</script>