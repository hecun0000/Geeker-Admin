<!-- 经典布局 -->
<template>
	<el-container style="min-width: 1030px">
		<el-header>
			<div class="header-lf">
				<div class="logo flx-center">
					<img src="@/assets/images/logo.svg" alt="logo" />
					<span>Geeker Admin</span>
				</div>
			</div>
			<ToolBarRight />
		</el-header>
		<el-container style="height: calc(100% - 55px)">
			<el-aside>
				<div class="menu" :style="{ width: isCollapse ? '65px' : '220px' }">
					<el-scrollbar>
						<el-menu
							:default-active="activeMenu"
							:router="false"
							:collapse="isCollapse"
							:collapse-transition="false"
							:unique-opened="true"
							background-color="#ffffff"
							text-color="#303133"
						>
							<SubMenu :menuList="menuList" />
						</el-menu>
					</el-scrollbar>
				</div>
			</el-aside>
			<el-container>
				<Tabs v-if="themeConfig.tabs" />
				<el-main>
					<router-view v-slot="{ Component }">
						<transition appear name="fade-transform" mode="out-in">
							<keep-alive :include="cacheRouter">
								<component :is="Component" :key="route.path" />
							</keep-alive>
						</transition>
					</router-view>
				</el-main>
				<el-footer v-if="themeConfig.footer">
					<Footer />
				</el-footer>
			</el-container>
		</el-container>
	</el-container>
</template>

<script setup lang="ts" name="layoutTransverse">
import { computed } from "vue";
import { useRoute } from "vue-router";
import { GlobalStore } from "@/store";
import { MenuStore } from "@/store/modules/menu";
import cacheRouter from "@/routers/cacheRouter";
import Tabs from "@/layouts/components/Tabs/index.vue";
import Footer from "@/layouts/components/Footer/index.vue";
import ToolBarRight from "@/layouts/components/Header/ToolBarRight.vue";
import SubMenu from "@/layouts/components/Menu/SubMenu.vue";

const route = useRoute();
const menuStore = MenuStore();
const globalStore = GlobalStore();
const activeMenu = computed(() => route.path);
const menuList = computed(() => menuStore.menuList);
const themeConfig = computed(() => globalStore.themeConfig);
const isCollapse = computed(() => menuStore.isCollapse);
</script>

<style scoped lang="scss">
@import "./index.scss";
</style>