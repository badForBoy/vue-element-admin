<template>
  <div>
    <el-card class="box-card" style="margin-top:40px;">
      <div slot="header" class="clearfix">
        <svg-icon icon-class="international" />
        <span style='margin-left:10px;'>{{$t('i18nView.title')}}</span>
      </div>
      <div>
        <el-radio-group v-model="lang" size="small">
          <el-radio label="zh" border>简体中文</el-radio>
          <el-radio label="en" border>English</el-radio>
        </el-radio-group>
        <el-tag style='margin-top:15px;display:block;' type="info">{{$t('i18nView.note')}}</el-tag>
      </div>
    </el-card>
  </div>
</template>

<script>
  import local from './local'
  const viewName = 'i18nView'

  export default {
    data() {
      return {}
    },
    computed: {
      lang: {
        get() {
          return this.$store.state.app.language
        },
        set(lang) {
          this.$i18n.locale = lang
          this.$store.dispatch('setLanguage', lang)
        }
      }
    },
    created() {
      if (!this.$i18n.getLocaleMessage('en')[viewName]) {
        this.$i18n.mergeLocaleMessage('en', local.en)
        this.$i18n.mergeLocaleMessage('zh', local.zh)
      }
    }
  }
</script>

<style rel="stylesheet/scss" lang="scss" scoped>
</style>
