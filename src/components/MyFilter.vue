<template>
  <aside class="app-filters">
    <section class="toggle-group">
      <button
        v-for="filter in filters"
        :key="filter"
        class="button"
        :class="{ 'button--primary': activeFilter === filter }"
        @click="setFilter(filter)"
      >
        {{ filter }}
      </button>
    </section>
  </aside>
</template>

<script lang="ts">
import { Filter } from "@/filter";
import { PropType, defineComponent } from "vue";
interface State {
  filters: Filter[];
}

export default defineComponent({
  props: {
    activeFilter: {
      type: String as PropType<Filter>,
    },
  },
  data(): State {
    return {
      filters: ["Active", "All", "Done"],
    };
  },
  methods: {
    setFilter(filter: Filter) {
      this.$emit("setFilter", filter);
    },
  },
  emits: {
    setFilter: (filter:Filter) => filter,
  },
});
</script>
