<template>
  <div>
    <md-layout>
      <md-layout>
        <span v-if="app" class="md-subheading">{{app.id}}</span>
      </md-layout>
      <md-layout md-align="end">
        <service-progress v-if="app" :app="app"></service-progress>
      </md-layout>
    </md-layout>
    <div v-if="!taskId">
      <md-button class="md-raised md-primary" v-on:click="move('serviceTaskList')">Tasks</md-button>
      <md-button class="md-raised md-primary" v-on:click="move('serviceConfiguration')">Configuration</md-button>
      <md-button class="md-raised md-primary" v-on:click="move('serviceDebug')">Debug</md-button>
    </div>
    <br>
    <router-view></router-view>
  </div>
</template>
<script>
  import DcosDataProvider from '../DcosDataProvider'
  import PathProvider from '../PathProvider'

  export default {
    mixins: [DcosDataProvider, PathProvider],
    props: {},
    data() {
      return {
        app: null
      }
    },
    mounted() {
      var me = this;
      me.init();
      window.busVue.$on('marathonApp', function (event) {
        var marathonAppId = event.body.app.id;
        if (me.app && marathonAppId == me.app.id) {
          me.app = event.body.app;
        }
      });
    },
    watch: {},
    methods: {
      init: function () {
        var me = this;
        me.getMarathonAppById(me.appId, function (response) {
          me.app = response.data.app;
        });
      },
      move: function (routeName) {
        var me = this;
        this.$router.push(
          {
            name: routeName,
            params: {appId: me.appId}
          }
        )
      }
    }
  }
</script>

<style scoped lang="scss" rel="stylesheet/scss">

</style>
