<template>
  <div class="container">
    <AdminCheckList v-if="this.$store.state.eventList" />
    <AdminCheckDetails v-else />
  </div>
</template>


<script>
import AdminCheckList from "./Admin-check-list";
import AdminCheckDetails from "./Admin-check-details";

export default {
  name: "Admin-check",
  components: {
    AdminCheckList,
    AdminCheckDetails
  },
  data() {
    return {};
  },
  computed: {},
  methods: {},
  mounted() {}
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
