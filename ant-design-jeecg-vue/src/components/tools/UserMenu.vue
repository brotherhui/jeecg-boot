<template>
  <div class="user-wrapper" :class="theme">
    <a-dropdown>
      <span class="action action-full ant-dropdown-link user-dropdown-menu">
        <a-avatar class="avatar" size="small" :src="getAvatar()"/>
        <span v-if="isDesktop()">欢迎您，{{ nickname() }}</span>
      </span>
    </a-dropdown>
    <span class="action">
      <a class="logout_title" href="javascript:;" @click="handleLogout">
        <a-icon type="logout"/>
        <span v-if="isDesktop()">&nbsp;退出登录</span>
      </a>
    </span>
  </div>
</template>

<script>
  import { mapActions, mapGetters } from 'vuex'
  import { mixinDevice } from '@/utils/mixin.js'
  export default {
    name: "UserMenu",
    mixins: [mixinDevice],
    props: {
      theme: {
        type: String,
        required: false,
        default: 'dark'
      }
    },
    methods: {
      ...mapActions(["Logout"]),
      ...mapGetters(["nickname", "avatar"]),
      getAvatar(){
        console.log('url = '+ window._CONFIG['imgDomainURL']+"/"+this.avatar())
        return window._CONFIG['imgDomainURL']+"/"+this.avatar()
      },
      handleLogout() {
        const that = this

        this.$confirm({
          title: '提示',
          content: '真的要注销登录吗 ?',
          onOk() {
            return that.Logout({}).then(() => {
                window.location.href="/";
              //window.location.reload()
            }).catch(err => {
              that.$message.error({
                title: '错误',
                description: err.message
              })
            })
          },
          onCancel() {
          },
        });
      },
    }
  }
</script>

<style scoped>
  .logout_title {
    color: inherit;
    text-decoration: none;
  }
</style>