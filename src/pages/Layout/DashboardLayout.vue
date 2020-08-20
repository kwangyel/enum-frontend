<template>
  <div class="wrapper" :class="{ 'nav-open': $sidebar.showSidebar }">
    <notifications></notifications>

    <side-bar
      :title="enumerate"
      :sidebar-item-color="sidebarBackground"
      :sidebar-background-image="sidebarBackgroundImage"
    >
      <mobile-menu slot="content"></mobile-menu>
      <sidebar-link to="/dashboard">
        <md-icon>dashboard</md-icon>
        <p>Dashboard</p>
      </sidebar-link>


      <div>
        <md-autocomplete
          class="search"
          v-model="selectedBlock"
          :md-options="blocks" >
          <label>Select Block</label>
        </md-autocomplete>
      </div>
      
    </side-bar>

    <div class="main-panel">
      <top-navbar></top-navbar>
      <!-- <AttributeTable/> -->
      <!-- <fixed-plugin
        :color.sync="sidebarBackground"
        :image.sync="sidebarBackgroundImage"
      >
      </fixed-plugin> -->
      <dashboard-content> </dashboard-content>
      <Map v-on:show-attrib="showAttrib" v-bind:block="selectedBlock"></Map>
      <AttributeTable v-bind:apidata="testapi"></AttributeTable>

      <content-footer v-if="!$route.meta.hideFooter"></content-footer>
    </div>
  </div>
</template>

<script>
import TopNavbar from "./TopNavbar.vue";
import ContentFooter from "./ContentFooter.vue";
import DashboardContent from "./Content.vue";
import MobileMenu from "@/pages/Layout/MobileMenu.vue";
// import FixedPlugin from "./Extra/FixedPlugin.vue";
import Map from "@/pages/Map.vue";
import AttributeTable from '../AttributeTable.vue';

export default {
  components: {
    TopNavbar,
    DashboardContent,
    ContentFooter,
    MobileMenu,
    AttributeTable,
    // FixedPlugin,
    Map
  },
  data() {
    return {
      enumerate: "Enumerate",
      sidebarBackground: "green",
      testapi: '',
      sidebarBackgroundImage: require("@/assets/img/sidebar-2.jpg"),
      selectedBlock: '',
      blocks: [
        "1",
        "2",
        "3",
        "4",
        "5",
        "6",
        "6",
        "7",
        "8",
        "9",
        "10",
        "11",
        "12",
        "13",
        "14",
        "15",
        "16",
        "17",
        "18",
        "19",
        "20",
        "21",
        "22",
        "23",
        "24",
        "25",
        "26",
        "27",
        "28",
        "29",
        "30"
      ]
    };
  },
  methods:{
    async actionBlock(){
      alert("test chnage")
    },
    async getInfo(bid){
      self = this
      this.$http.post('http://43.230.208.80:8080/enum/get-building',{
          bid: bid
      })
      .then(function(resp){
        self.testapi=[]
        self.testapi.push(resp.data.data)
      }).catch(function(error){
        self.testapi = [] 
      })
    },
    showAttrib(e){
      this.getInfo(e)
    }

  }
};
</script>
