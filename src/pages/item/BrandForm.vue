<template>
  <v-form v-model="valid" ref="brandForm">
    <v-text-field v-model="brand.name" label="请输入品牌名称" required :rules="nameRules"/>
    <v-text-field v-model="brand.letter" label="请输入品牌首字母" required :rules="letterRules"/>
    <v-cascader
      url="/item/category/list"
      multiple
      required
      v-model="brand.categories"
      label="请选择商品分类"/>
    <v-layout row>
      <v-flex xs3>
        <span style="font-size: 16px; color: #444">品牌LOGO：</span>
      </v-flex>
      <v-flex>
        <v-upload
          v-model="brand.image"
          url="/upload/image"
          :multiple="false"
          :pic-width="250"
          :pic-height="90"
        />
      </v-flex>
    </v-layout>
    <v-layout class="my-4" row>
      <v-spacer/>
      <v-btn @click="submit" color="primary">提交</v-btn>
      <v-btn @click="clear">重置</v-btn>
    </v-layout>
  </v-form>
</template>

<script>
  export default {
    name: "BrandForm",
    props: {
      oldBrand: {
        type: Object,
      },
      isEdit: {
        type: Boolean,
        default: false,
      }
    },
    data() {
      return {
        // 表单效验结果标识
        valid: false,
        brand: {
          name: '',
          letter: '',
          image: '',
          // 品牌所属的商品分类数组
          categories: [],
        },
        nameRules: [
          v => !!v || '品牌不能为空',
          v => (v && v.length <= 10) || '品牌名称不能大于10'
        ],
        letterRules: [
          v => !!v || '首字母不能为空',
          v => /^[A-Z]{1}$/.test(v) || '品牌字母只能是A~Z的大写字母'
        ],
      }
    },

    methods: {
      submit() {
        // 表单验证
        if (this.$refs.brandForm.validate()) {
          const {categories, letter, ...params} = this.brand
          // 分类id
          params.cids = categories.map(c => c.id).join(',')
          params.letter = letter.toUpperCase()
          this.$http({
            method: this.isEdit ? 'put' : 'post',
            url: '/item/brand',
            data: this.$qs.stringify(params),
          })
            .then(() => {
              this.$emit('close')
              this.$message.success("保存成功")
            })
            .catch((e) => {
              console.log(e)
              this.$message.error("保存失败")
            })
        }
      },
      clear() {
        // 重置表单
        this.$refs.brandForm.reset()
      }
    },

    watch: {
      oldBrand: {
        handler(val) {
          if (val) {
            // 注意不要直接复制，否则这边的修改会影响到父组件的数据，copy属性即可
            this.brand = Object.deepCopy(val)
          } else {
            // 为空，初始化brand
            this.brand = {
              name: '',
              letter: '',
              image: '',
              categories: [],
            }
          }
        },
        deep: true
      }
    }

  }
</script>

<style scoped>

</style>
