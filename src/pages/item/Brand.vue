<template>
  <div>
    <v-layout row class="pb-2 px-2">
      <v-flex xs3>
        <v-btn color="success">新增品牌</v-btn>
      </v-flex>
      <v-spacer/>
      <v-flex xs5>
        <v-text-field label="品牌搜索" append-icon="search" hide-details="true" v-model="name"/>
      </v-flex>
    </v-layout>
    <v-data-table
      :headers="headers"
      :items="brands"
      :pagination.sync="pagination"
      :total-items="totalBrands"
      :loading="loading"
      class="elevation-1"
    >
      <template slot="items" slot-scope="props">
        <td class="text-xs-center">{{ props.item.id }}</td>
        <td class="text-xs-center">{{ props.item.name }}</td>
        <td class="text-xs-center"><img :src="props.item.image" alt=""></td>
        <td class="text-xs-center">{{ props.item.letter }}</td>
        <td class="text-xs-center">
          <v-btn flat icon color="info">
            <v-icon>edit</v-icon>
          </v-btn>
          <v-btn flat icon color="red">
            <v-icon>delete</v-icon>
          </v-btn>
        </td>
      </template>
    </v-data-table>
  </div>
</template>

<script>
  export default {
    name: "Brand",
    data() {
      return {
        headers: [
          {text: 'Brand Id', align: 'center', sortable: true, value: 'id',},
          {text: '品牌名称', align: 'center', sortable: false, value: 'name',},
          {text: '品牌LOGO', align: 'center', sortable: false, value: 'image',},
          {text: '品牌首字母', align: 'center', sortable: true, value: 'letter',},
          {text: '操作', align: 'center',},
        ],
        brands: [],
        pagination: {},
        totalBrands: 0,
        loading: false,
        name: '',
      }
    },
    created() {
      this.brands = [
        {
          "id": 2032,
          "name": "OPPO",
          "image": "http://img10.360buyimg.com/popshop/jfs/t2119/133/2264148064/4303/b8ab3755/56b2f385N8e4eb051.jpg",
          "letter": "O"
        },
        {
          "id": 2033,
          "name": "飞利浦（PHILIPS）",
          "image": "http://img12.360buyimg.com/popshop/jfs/t18361/122/1318410299/1870/36fe70c9/5ac43a4dNa44a0ce0.jpg",
          "letter": "F"
        },
        {
          "id": 2034,
          "name": "华为（HUAWEI）",
          "image": "http://img10.360buyimg.com/popshop/jfs/t5662/36/8888655583/7806/1c629c01/598033b4Nd6055897.jpg",
          "letter": "H"
        },
        {
          "id": 2036,
          "name": "酷派（Coolpad）",
          "image": "http://img10.360buyimg.com/popshop/jfs/t2521/347/883897149/3732/91c917ec/5670cf96Ncffa2ae6.jpg",
          "letter": "K"
        },
        {
          "id": 2037,
          "name": "魅族（MEIZU）",
          "image": "http://img13.360buyimg.com/popshop/jfs/t3511/131/31887105/4943/48f83fa9/57fdf4b8N6e95624d.jpg",
          "letter": "M"
        }
      ]
      this.totalBrands = 50
    },
    methods: {
      loadBrands() {
        this.$http.get('/brand/list', {
          params: {
            name: this.name,
            desc: this.pagination.descending,
            page: this.pagination.page,
            rowsPerPage: this.pagination.rowsPerPage,
            sortBy: this.pagination.sortBy,
          }
        })
      }
    },
    watch: {
      name: {
        handler() {
          this.loadBrands()
        }
      },
      pagination: {
        deep: true,
        handler() {
          this.loadBrands()
        }
      }
    }
  }
</script>

<style scoped>

</style>
