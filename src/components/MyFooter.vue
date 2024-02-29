<template>
  <footer class="app-footer">
    {{ stats.activeTasks }} more to do, {{ stats.doneTasks }} done
  </footer>
</template>

<script lang="ts">
import { PropType, defineComponent } from "vue";

export interface Stats {
  activeTasks: number;
  doneTasks: number;
}

export default defineComponent({
  props: {
    stats: {
      type: Object as PropType<Stats>,
      required: true,
    },
  },
});
</script>
