<script setup lang="ts">
import DefaultTheme, { VPSocialLink } from 'vitepress/theme'
import { useData } from 'vitepress'
import { computed } from 'vue'
// @ts-expect-error
import { useSidebar } from 'vitepress/dist/client/theme-default/composables/sidebar'

const { Layout } = DefaultTheme

const {
  hasSidebar,
  isOpen,
  // open: openSidebar,
  // close: closeSidebar,
} = useSidebar()

const data = useData()
const base = computed(() => data.site.value.base)

function openSidebar() {
  const el = document.querySelector('div.VPLocalNav.has-sidebar button') as any;
  el.click();
}

function closeSidebar() {
  const el = document.querySelector('.VPBackdrop.backdrop') as any;
  el.click();
}

const touchStart = { x: 0, y: 0 };

const onTouchStart = (e: TouchEvent): void => {
  touchStart.x = e.changedTouches[0].clientX;
  touchStart.y = e.changedTouches[0].clientY;
};

const onTouchEnd = (e: TouchEvent): void => {
  const dx = e.changedTouches[0].clientX - touchStart.x;
  const dy = e.changedTouches[0].clientY - touchStart.y;
  if (Math.abs(dx) > Math.abs(dy) && Math.abs(dx) > 40) {
    if (dx > 0 && touchStart.x <= 80) {
      // openSidebar();
    } else {
      closeSidebar();
    }
  }
};
</script>

<template>
  <Layout @touchstart="onTouchStart" @touchend="onTouchEnd">
    <template #nav-bar-title-before>
      <button
        v-if="hasSidebar"
        class="menu"
        :aria-expanded="isOpen"
        aria-controls="VPSidebarNav"
        @click.prevent="openSidebar()"
      >
        <span class="vpi-align-left menu-icon"></span>
      </button>
    </template>
    <template #sidebar-nav-after>
      <div style="flex-grow: 1" />
      <div>
        <VPSocialLink
          icon="github"
          :link="`https://github.com/OSH-2025${base}`"
        />
      </div>
    </template>
  </Layout>
</template>

<style scoped>
@media (min-width: 960px) {
  .menu {
    display: none !important;
  }
}

.menu {
  display: flex;
  padding-right: 12px;
  padding-bottom: 2px;
  color: var(--vp-c-text-2);
  transition: color 0.5s;
}

.menu:hover {
  color: var(--vp-c-text-1);
  transition: color 0.25s;
}
</style>
