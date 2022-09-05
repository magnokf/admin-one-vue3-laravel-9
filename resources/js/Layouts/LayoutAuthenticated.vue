<script setup>
import { Inertia } from "@inertiajs/inertia";
import { mdiForwardburger, mdiBackburger, mdiMenu } from "@mdi/js";
import menuAside from "@/menuAside.js";
import menuNavBar from "@/menuNavBar.js";
import { useMainStore } from "@/Stores/main.js";
import { useLayoutStore } from "@/Stores/layout.js";
import { useStyleStore } from "@/Stores/style.js";
import BaseIcon from "@/Components/BaseIcon.vue";
import FormControl from "@/Components/FormControl.vue";
import NavBar from "@/Components/NavBar.vue";
import NavBarItemPlain from "@/Components/NavBarItemPlain.vue";
import AsideMenu from "@/Components/AsideMenu.vue";
import FooterBar from "@/Components/FooterBar.vue";

Inertia.on('navigate', () => {
    layoutStore.isAsideMobileExpanded = false
    layoutStore.isAsideLgActive = false
})

useMainStore().setUser({
  name: "John Doe",
  email: "john@example.com",
  avatar:
    "https://avatars.dicebear.com/api/avataaars/example.svg?options[top][]=shortHair&options[accessoriesChance]=93",
});

const layoutAsidePadding = "xl:pl-60";

const styleStore = useStyleStore();

const layoutStore = useLayoutStore();





const menuClick = (event, item) => {
  if (item.isToggleLightDark) {
    styleStore.setDarkMode();
  }

  if (item.isLogout) {
    //
      Inertia.post(route('logout'));
  }
};
</script>

<template>
  <div
    :class="{
      dark: styleStore.darkMode,
      'overflow-hidden lg:overflow-visible': layoutStore.isAsideMobileExpanded,
    }"
  >
    <div
      :class="[
        layoutAsidePadding,
        { 'ml-60 lg:ml-0': layoutStore.isAsideMobileExpanded },
      ]"
      class="pt-14 min-h-screen w-screen transition-position lg:w-auto bg-gray-50 dark:bg-slate-800 dark:text-slate-100"
    >
      <NavBar
        :menu="menuNavBar"
        :class="[
          layoutAsidePadding,
          { 'ml-60 lg:ml-0': layoutStore.isAsideMobileExpanded },
        ]"
        @menu-click="menuClick"
      >
        <NavBarItemPlain
          display="flex lg:hidden"
          @click.prevent="layoutStore.asideMobileToggle()"
        >
          <BaseIcon
            :path="
              layoutStore.isAsideMobileExpanded
                ? mdiBackburger
                : mdiForwardburger
            "
            size="24"
          />
        </NavBarItemPlain>
        <NavBarItemPlain
          display="hidden lg:flex xl:hidden"
          @click.prevent="layoutStore.isAsideLgActive = true"
        >
          <BaseIcon :path="mdiMenu" size="24" />
        </NavBarItemPlain>
        <NavBarItemPlain use-margin>
          <FormControl
            placeholder="Search (ctrl+k)"
            ctrl-k-focus
            transparent
            borderless
          />
        </NavBarItemPlain>
      </NavBar>
      <AsideMenu :menu="menuAside" @menu-click="menuClick" />
      <slot />
      <FooterBar>
        Get more with
        <a
          href="https://tailwind-vue.justboil.me/"
          target="_blank"
          class="text-blue-600"
          >Premium version</a
        >
      </FooterBar>
    </div>
  </div>
</template>
