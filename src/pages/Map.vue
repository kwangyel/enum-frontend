<template>
  <div style="height:500px;width:100%">
    <l-map
      :zoom.sync="zoom"
      :center="center"
      :options="mapOptions"
      style="height: 100%"
      @update:center="centerUpdate"
      @update:zoom="zoomUpdate">
      <l-tile-layer :url="url" />
      <!-- <l-geo-json v-if="zoom > 9" -->
        <l-geo-json
        :geojson="geojson"
        :options="mapoptions"
      />
    </l-map>

  </div>
</template>

<script>
import { latLng } from "leaflet";
import { LMap, LTileLayer, LMarker,LGeoJson  } from "vue2-leaflet";
import { Icon } from 'leaflet';
import 'leaflet/dist/leaflet.css';

delete Icon.Default.prototype._getIconUrl;
Icon.Default.mergeOptions({
  iconRetinaUrl: require('leaflet/dist/images/marker-icon-2x.png'),
  iconUrl: require('leaflet/dist/images/marker-icon.png'),
  shadowUrl: require('leaflet/dist/images/marker-shadow.png'),
});

var myicon = L.icon({
    iconUrl: "http://localhost:8080/icon.png",
    iconSize: [15,15]
});

var myicon2 = L.icon({
    iconUrl: "http://localhost:8080/icon2.png",
    iconSize: [15,15]
});
export default {
  name: 'Map',
  components: {
    LMap,
    LTileLayer,
    LGeoJson    
  },
  props:['block'],
  computed: {
    styleFunction() {
      const fillColor = this.fillColor; // important! need touch fillColor in computed for re-calculate when change fillColor
      return () => {
        return {
          weight: 2,
          color:"blue",
          opacity: 1,
          fillColor: fillColor,
          fillOpacity: 0.0
        };
      };
    },
  },
  data () {
    return {
      withdata:[],
      info:'',
      geojson: null,
      enableTooltip: true,
      msg: 'Welcome to Bhutan Maps.',
      dzoThromdes:[],
      zoom: 12,
      center: latLng( 27.4712,89.6339),
      url: "http://{s}.tile.osm.org/{z}/{x}/{y}.png",
      mapOptions: {
        zoomSnap: 0.5
      },
      mapoptions:{ 
          onEachFeature: (feature,layer) => {
            const self = this
            layer.on('click',function(e){
              self.$emit("show-attrib",feature.properties.OBJECTID_1)
            })
            layer.bindTooltip(
                "<div>Building ID:" +
                  feature.properties.OBJECTID_1+
                  "</div>",
                { permanent: false, sticky: true }
            )
          }, 
          pointToLayer: (feature,latlng) => {
            const v = this
            if(v.withdata.length != 0){
              if(v.withdata.includes(feature.properties.OBJECTID_1)){
                return L.marker(latlng,{icon:myicon2})
              }
            }
            return L.marker(latlng,{icon:myicon})
          }
        },
      }
  },
  methods: {
    async getAllBldg(){
      var self = this
      this.$http.get('http://43.230.208.80:8080/enum/get-all-bid')
      .then(function(resp){
        for(var i = 0; i < resp.data.data.length; i++){
          // console.log(resp.data.data[i]['bldg_id'])
          self.withdata.push(resp.data.data[i]['bldg_id'])
        }
      }).catch(function(error){
        console.log(error)
      })
    },
    async getInfo(bid){
      self = this
      this.$http.post('http://43.230.208.80:8080/enum/get-building',{
          bid: bid
      })
      .then(function(resp){
        self.testapi.push(resp.data.data)
      }).catch(function(error){
        self.testapi = [] 
      })
    },
    async getBlockBldg(block){
      if(block!=""){
        try{
          this.$http.get("http://localhost:8080/"+block+".geojson")
          .then((resp)=>{
            console.log(resp)
            this.geojson = resp.data
          })
        }catch(error){
          console.log(error)
        }
      }
    },
    zoomUpdate(zoom) {
      currentZoom = zoom;
    },
    centerUpdate(center) {
      this.currentCenter = center;
    },
  },
  mounted: function(){
    // this.getDzoThromdes();
    // this.getBuildings();
    this.getAllBldg()
    this.getBlockBldg(this.$props.block)
  },
  watch:{
    block: function(val){
      this.getBlockBldg(val)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
#map {
  width: 100%;
  height: 100%;
}
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
