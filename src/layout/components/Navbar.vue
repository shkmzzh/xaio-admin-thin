<script setup lang="ts">
import { storeToRefs } from 'pinia'
import { useRoute, useRouter } from 'vue-router'
import { useAppStore } from '@/store/modules/app'
import { useTagsViewStore } from '@/store/modules/tagsView'
import { useUserStore } from '@/store/modules/user'
import { useSettingsStore } from '@/store/modules/settings'
import RightPanel from '@/components/RightPanel/index.vue'
import Setting from '@/layout/components/Settings/index.vue'
import { ref, onMounted } from 'vue'
import { useWindowSize } from '@vueuse/core'
import { SwitchButton } from '@element-plus/icons-vue'

const { width } = useWindowSize()
const bol = ref<number>()
onMounted(() => {
  window.addEventListener('resize', function handleResize() {
    width.value = window.innerWidth
  })
})

const settingsStore = useSettingsStore()
const showSettings = computed(() => settingsStore.showSettings)
const appStore = useAppStore()
const tagsViewStore = useTagsViewStore()
const userStore = useUserStore()

const route = useRoute()
const router = useRouter()

const { device } = storeToRefs(appStore) // 设备类型：desktop-宽屏设备 || mobile-窄屏设备

function toggleSideBar() {
  appStore.toggleSidebar(true)
}

// 设置面板
const show = ref(false)
</script>

<template>
  <div class="navbar">
    <div class="flex items-center">
      <hamburger :is-active="appStore.sidebar.opened" @toggleClick="toggleSideBar" />
      <breadcrumb />
    </div>
    <div class="flex items-center">
      <div v-if="device !== 'mobile'" class="flex items-center">
        <screenfull class="navbar-setting-item  screenfull" />
        <!-- 布局大小 -->
        <!-- <el-tooltip content="布局大小" effect="dark" placement="bottom">
          <size-select class="navbar-setting-item" />
        </el-tooltip> -->
        <!--语言选择-->
        <lang-select class="navbar-setting-item" />
        <el-dropdown trigger="click" class="dropdown">
          <span class="el-dropdown-link navbar-bg-hover select-none">
            <img src="https://pic3.zhimg.com/80/v2-738a80bf6bfd7adc2a30afc1b3937f34_r.jpg" />
            <p class="out">admin</p>
          </span>
          <template #dropdown>
            <el-dropdown-menu class="logout">
              <el-dropdown-item @click="userStore.outLogin">
                <el-icon>
                  <SwitchButton />
                </el-icon>
                退出系统
              </el-dropdown-item>
            </el-dropdown-menu>
          </template>
        </el-dropdown>
      </div>
      <div class="setting" @click.stop="show = true">
        <i-ep-setting />
      </div>
    </div>
  </div>
  <RightPanel v-model:show="show">
    <Setting></Setting>
  </RightPanel>
</template>

<style lang="scss" scoped>
.navbar {
  background-color: #fff;
  height: 50px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  box-shadow: 0 0 1px #0003;

  .navbar-setting-item,
  .setting {
    height: 50px;
    line-height: 50px;
    width: 32px;
    display: inline-block;
    cursor: pointer;
    text-align: center;
    color: #39393a;

    &:hover {
      background: rgb(244, 245, 246);
    }
  }
  .screenfull{
    font-size: 17px;
  }

  .setting {
    width: 45px;
    line-height: 60px;
    font-size: 15px;
  }
  .el-dropdown-link {
    display: flex;
    align-items: center;
    justify-content: space-around;
    height: 50px;
    padding: 10px;

    cursor: pointer;

    &:hover {
      background: rgb(244, 245, 246);
    }

    p {
      font-size: 14px;
      margin-left: 7px;
      color: #000000d9;
    }

    img {
      width: 22px;
      height: 22px;
      border-radius: 50%;
    }
  }
}</style>
