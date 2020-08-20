<template>
<div>
  <div>
    <md-table v-model="apidata" :table-header-color="tableHeaderColor">
      <md-table-row slot="md-table-row" slot-scope="{ item }">
        <md-table-cell md-label="Building ID">{{ item.bldg_id}}</md-table-cell>
        <md-table-cell md-label="Building Name">{{ item.name_bldg}}</md-table-cell>
        <md-table-cell md-label="Owner name">{{ item.name_own}}</md-table-cell>
        <md-table-cell md-label="Plot no.">{{ item.plot_no}}</md-table-cell>
        <md-table-cell md-label="Thram">{{ item.year_const}}</md-table-cell>
        <md-table-cell md-label="Thram">{{ item.year_renov}}</md-table-cell>
        <md-table-cell md-label="Thram">{{ item.thram}}</md-table-cell>
        <md-table-cell md-label="floors">{{item.floors}}</md-table-cell>
        <md-table-cell md-label="attic">{{item.attic}}</md-table-cell>
        <md-table-cell md-label="basement">{{item.basement}}</md-table-cell>
        <md-table-cell md-label="exist_status">{{item.exist_status}}</md-table-cell>
        <md-table-cell md-label="asso_pos">{{item.asso_pos}}</md-table-cell>
        <md-table-cell md-label="facade">{{item.facade}}</md-table-cell>
        <md-table-cell md-label="str_type">{{item.str_type}}</md-table-cell>
        <md-table-cell md-label="mt_type">{{item.mt_type}}</md-table-cell>
        <md-table-cell md-label="roof_mt">{{item.roof_mt}}</md-table-cell>
        <md-table-cell md-label="roof_type">{{item.roof_type}}</md-table-cell>
        <md-table-cell md-label="eme_exit">{{item.eme_exit}}</md-table-cell>
        <md-table-cell md-label="lift">{{item.lift}}</md-table-cell>
        <md-table-cell md-label="sewer">{{item.sewer}}</md-table-cell>
        <md-table-cell md-label="waste">{{item.waste}}</md-table-cell>
        <md-table-cell md-label="waste_coll">{{item.waste_coll}}</md-table-cell>
        <md-table-cell md-label="water">{{item.water}}</md-table-cell>
        <md-table-cell md-label="bldg_use">{{item.bldg_use}}</md-table-cell>
      </md-table-row>
    </md-table>
  </div>
</div>
</template>

<script>
export default {
  name: 'AttributeTable',
  props: {
    tableHeaderColor: {
      type: String,
      default: ""
    },
    apidata:{
      type: Array,
      default:function(){return []}
    }
  },
  computed: {
  },
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
