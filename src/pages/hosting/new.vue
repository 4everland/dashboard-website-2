<style lang="scss">
.new-project {
  .v-window {
    overflow: visible;
  }
  .main-wrap {
    min-height: auto;
    padding: 20px;
  }
}
</style>

<template>
  <div class="new-project">
    <e-steps :options="stepList" :value="curStep"></e-steps>
    <!-- <div class="main-wrap">new</div> -->
    <v-window v-model="curStep" class="mt-5">
      <v-window-item v-for="i in [0, 1, 2]" :key="i" :value="i">
        <component
          :is="'new-step-' + i"
          :active="i == curStep"
          :data="info"
          @set-info="info = $event"
          @next="curStep += 1"
          @back="onBack"
        />
      </v-window-item>
    </v-window>
  </div>
</template>

<script>
import NewStep0 from "@/views/hosting/new/new-step-0";
import NewStep1 from "@/views/hosting/new/new-step-1";
import NewStep2 from "@/views/hosting/new/new-step-2";

export default {
  data() {
    return {
      stepList: ["Select Repository", "Edit Configuration", "Start Deployment"],
      curStep: 0,
      info: null,
    };
  },
  watch: {
    "$route.path"() {
      this.curStep = 0;
    },
    "$route.query"() {
      this.onQuery();
    },
  },
  mounted() {
    this.onQuery();
  },
  methods: {
    onQuery() {
      const { taskId, type, s } = this.$route.query;
      if (type == "clone-flow" && s) this.curStep = 1;
      else if (taskId) this.curStep = 2;
    },
    onBack() {
      this.$router.back();
      this.curStep -= 1;
    },
  },
  components: {
    NewStep0,
    NewStep1,
    NewStep2,
  },
};
</script>