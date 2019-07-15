<template>
  <div>
    <v-layout row class="pb-2 px-2">
      <v-flex xs3>
        <v-btn color="success" @click="addBrand">新增品牌</v-btn>
      </v-flex>
      <v-spacer/>
      <v-flex xs5>
        <v-text-field label="品牌搜索" append-icon="search" :hide-details="true" v-model="name"/>
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
    <!--对话框-->
    <v-dialog max-width="600" persistent v-model="show">
      <v-card>
        <!--对话框的标题-->
        <v-toolbar dense dark color="primary">
          <v-toolbar-title>新增品牌</v-toolbar-title>
          <v-spacer/>
          <v-btn icon @click="closeWindow">
            <v-icon>close</v-icon>
          </v-btn>
        </v-toolbar>
        <!--对话框的内容，表单-->
        <v-card-text class="px-5">
          <brand-form/>
        </v-card-text>
      </v-card>
    </v-dialog>
  </div>
</template>

<script>

  import BrandForm from "./BrandForm";

  export default {
    name: "Brand",
    components: {BrandForm},
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
        // 控制对话框是否显示
        show: false,
      }
    },
    created() {
      this.loadBrands()
    },

    methods: {
      loadBrands() {
        this.loading = true
        this.$http.get('/item/brand/list', {
          params: {
            name: this.name,
            desc: this.pagination.descending,
            page: this.pagination.page,
            rowsPerPage: this.pagination.rowsPerPage,
            sortBy: this.pagination.sortBy,
          }
        }).then(resp => {
          this.brands = resp.data.items
          this.totalBrands = resp.data.total
          // 完成赋值后，把加载状态赋值为false
          this.loading = false
        })
      },
      addBrand() {
        this.show = true
      },
      closeWindow() {
        this.show = false
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
