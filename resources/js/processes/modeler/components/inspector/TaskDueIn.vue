<template>
    <div class="form-group">
        <label>{{ $t("Due In") }}</label>
        <input class="form-control"
               type="number"
               :placeholder="$t('72 hours')"
               :value="dueInGetter"
               @input="dueInSetter"
               min="0"
               v-on:keydown="dueInValidate">
        <small class="form-text text-muted">{{ $t("Enter the hours until this Task is overdue") }}</small>
    </div>
</template>

<script>
  export default {
    props: ["value", "label", "helper", "property"],
    computed: {
      dueInGetter() {
        return _.get(this.node, "dueIn");
      },
      node() {
        return this.$root.$children[0].$refs.modeler.highlightedNode.definition;
      },
    },
    methods: {
      dueInValidate(event) {
        if (event.key === "-") {
          event.preventDefault();
          return;
        }
      },
      /**
       * Update due in property
       */
      dueInSetter(event) {
        const validValue = Math.abs(event.target.value * 1) || "";
        if (!validValue) {
          this.$delete(this.node, "dueIn");
        } else {
          this.$set(this.node, "dueIn", validValue);
        }
        this.$emit("input", this.value);
        String(validValue) !== event.target.value
          ? (event.target.value = validValue)
          : null;
      },
    }
  };
</script>

<style lang="scss" scoped>
</style>
