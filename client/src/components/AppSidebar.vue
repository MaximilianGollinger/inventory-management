<!--
  Sidebar navigation for the inventory management app.
  Persistent collapse state via localStorage ('sidebar-collapsed').
  On mobile (<768px) renders as an off-canvas drawer with a sticky topbar.
  Forwards show-profile-details and show-tasks events from ProfileMenu up to
  App.vue so modal openers remain functional.
-->
<template>
  <div class="sidebar-wrapper">
    <!-- Mobile top bar — visible only <768px -->
    <div class="mobile-topbar">
      <button
        class="hamburger"
        :aria-expanded="mobileOpen"
        :aria-label="mobileOpen ? 'Close navigation' : 'Open navigation'"
        @click="toggleMobile"
      >
        <Menu :size="20" />
      </button>
      <span class="mobile-brand">{{ t('nav.companyName') }}</span>
    </div>

    <!-- Backdrop for mobile drawer -->
    <div
      v-if="mobileOpen"
      class="mobile-backdrop"
      @click="closeMobile"
    />

    <aside
      class="sidebar"
      :class="{ collapsed, 'mobile-open': mobileOpen }"
    >
      <div class="sidebar-brand">
        <h1 class="brand-name">{{ t('nav.companyName') }}</h1>
        <span v-if="!collapsed" class="brand-subtitle">{{ t('nav.subtitle') }}</span>
      </div>

      <nav class="sidebar-nav" role="navigation">
        <router-link
          v-for="item in items"
          :key="item.to"
          :to="item.to"
          :title="t(item.label)"
          class="nav-item"
          @click="closeMobile"
        >
          <component :is="item.icon" :size="18" class="nav-icon" />
          <span v-if="!collapsed" class="nav-label">{{ t(item.label) }}</span>
        </router-link>
      </nav>

      <div class="sidebar-footer">
        <div v-if="!collapsed" class="footer-row">
          <LanguageSwitcher />
        </div>
        <div class="footer-row">
          <!-- Forward modal-trigger events so App.vue handlers still fire -->
          <ProfileMenu
            @show-profile-details="$emit('show-profile-details')"
            @show-tasks="$emit('show-tasks')"
          />
        </div>
        <button
          class="collapse-toggle"
          :aria-label="collapsed ? 'Expand sidebar' : 'Collapse sidebar'"
          @click="toggleCollapse"
        >
          <component :is="collapsed ? ChevronRight : ChevronLeft" :size="16" />
        </button>
      </div>
    </aside>
  </div>
</template>

<script>
import { ref, onMounted } from 'vue'
import {
  LayoutDashboard,
  Boxes,
  ShoppingCart,
  DollarSign,
  TrendingUp,
  FileText,
  ChevronLeft,
  ChevronRight,
  Menu
} from 'lucide-vue-next'
import { useI18n } from '../composables/useI18n'
import LanguageSwitcher from './LanguageSwitcher.vue'
import ProfileMenu from './ProfileMenu.vue'

export default {
  name: 'AppSidebar',

  components: {
    LanguageSwitcher,
    ProfileMenu,
    LayoutDashboard,
    Boxes,
    ShoppingCart,
    DollarSign,
    TrendingUp,
    FileText,
    ChevronLeft,
    ChevronRight,
    Menu
  },

  emits: ['show-profile-details', 'show-tasks'],

  setup() {
    const { t } = useI18n()

    const collapsed = ref(false)
    const mobileOpen = ref(false)

    onMounted(() => {
      collapsed.value = localStorage.getItem('sidebar-collapsed') === 'true'
    })

    const items = [
      { to: '/',          label: 'nav.overview',       icon: LayoutDashboard },
      { to: '/inventory', label: 'nav.inventory',      icon: Boxes },
      { to: '/orders',    label: 'nav.orders',         icon: ShoppingCart },
      { to: '/spending',  label: 'nav.finance',        icon: DollarSign },
      { to: '/demand',    label: 'nav.demandForecast', icon: TrendingUp },
      { to: '/reports',   label: 'nav.reports',        icon: FileText }
    ]

    const toggleCollapse = () => {
      collapsed.value = !collapsed.value
      localStorage.setItem('sidebar-collapsed', String(collapsed.value))
    }

    const toggleMobile = () => {
      mobileOpen.value = !mobileOpen.value
    }

    const closeMobile = () => {
      mobileOpen.value = false
    }

    return {
      t,
      collapsed,
      mobileOpen,
      items,
      toggleCollapse,
      toggleMobile,
      closeMobile,
      ChevronLeft,
      ChevronRight
    }
  }
}
</script>

<style scoped>
.sidebar-wrapper {
  display: contents;
}

/* ============================================================
 * Desktop sidebar
 * ============================================================ */
.sidebar {
  display: flex;
  flex-direction: column;
  width: var(--sidebar-width);
  background: var(--color-surface);
  border-right: 1px solid var(--color-border);
  position: sticky;
  top: 0;
  height: 100vh;
  z-index: var(--z-sidebar);
  transition: width var(--transition-base);
}

.sidebar.collapsed {
  width: var(--sidebar-width-collapsed);
}

.sidebar-brand {
  display: flex;
  flex-direction: column;
  padding: var(--space-5);
  min-height: 56px;
  border-bottom: 1px solid var(--color-border);
}

.brand-name {
  margin: 0;
  font-size: var(--text-xl);
  font-weight: var(--font-bold);
  letter-spacing: var(--tracking-tight);
  color: var(--color-text-primary);
  line-height: 1.2;
}

.brand-subtitle {
  font-size: var(--text-xs);
  color: var(--color-text-secondary);
  margin-top: var(--space-1);
}

.sidebar.collapsed .brand-name {
  font-size: var(--text-base);
  text-align: center;
}

.sidebar-nav {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: var(--space-1);
  padding: var(--space-3);
  overflow-y: auto;
}

.nav-item {
  display: flex;
  align-items: center;
  gap: var(--space-3);
  height: 36px;
  padding: var(--space-2) var(--space-3);
  border-radius: var(--radius-sm);
  color: var(--color-text-secondary);
  text-decoration: none;
  font-size: var(--text-sm);
  font-weight: var(--font-medium);
  transition:
    background-color var(--transition-base),
    color var(--transition-base);
}

.nav-item:hover {
  background: var(--slate-50);
  color: var(--color-text-primary);
}

.nav-item.router-link-active,
.nav-item.router-link-exact-active {
  background: var(--color-accent-bg);
  color: var(--color-accent);
}

.nav-icon {
  flex-shrink: 0;
}

.sidebar.collapsed .nav-item {
  justify-content: center;
  padding: var(--space-2);
}

.nav-label {
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.sidebar-footer {
  padding: var(--space-3);
  border-top: 1px solid var(--color-border);
  display: flex;
  flex-direction: column;
  gap: var(--space-2);
}

.footer-row {
  display: flex;
  align-items: center;
}

.sidebar.collapsed .footer-row {
  justify-content: center;
}

.collapse-toggle {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 32px;
  width: 100%;
  background: transparent;
  border: 1px solid var(--color-border);
  border-radius: var(--radius-sm);
  color: var(--color-text-secondary);
  cursor: pointer;
  transition:
    background-color var(--transition-base),
    color var(--transition-base);
}

.collapse-toggle:hover {
  background: var(--slate-50);
  color: var(--color-text-primary);
}

/* ============================================================
 * Mobile top bar
 * ============================================================ */
.mobile-topbar {
  display: none;
  align-items: center;
  gap: var(--space-3);
  height: var(--sidebar-mobile-topbar-height);
  padding: 0 var(--space-4);
  background: var(--color-surface);
  border-bottom: 1px solid var(--color-border);
  position: sticky;
  top: 0;
  z-index: var(--z-mobile-topbar);
}

.hamburger {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 32px;
  height: 32px;
  background: transparent;
  border: none;
  border-radius: var(--radius-sm);
  color: var(--color-text-primary);
  cursor: pointer;
}

.hamburger:hover {
  background: var(--slate-50);
}

.mobile-brand {
  font-size: var(--text-base);
  font-weight: var(--font-semibold);
  color: var(--color-text-primary);
}

.mobile-backdrop {
  display: none;
  position: fixed;
  inset: 0;
  background: rgba(0, 0, 0, 0.5);
  z-index: var(--z-drawer-backdrop);
}

/* ============================================================
 * Mobile (<768px)
 * ============================================================ */
@media (max-width: 768px) {
  .mobile-topbar {
    display: flex;
  }

  .sidebar {
    position: fixed;
    top: 0;
    left: 0;
    transform: translateX(-100%);
    width: var(--sidebar-width);
    z-index: calc(var(--z-drawer-backdrop) + 1);
    transition: transform var(--transition-base);
  }

  /* Mobile always opens full-width — collapsed prop is desktop-only */
  .sidebar.collapsed {
    width: var(--sidebar-width);
  }

  .sidebar.mobile-open {
    transform: translateX(0);
  }

  .sidebar.collapsed .brand-name {
    font-size: var(--text-xl);
    text-align: left;
  }

  .sidebar.collapsed .nav-item {
    justify-content: flex-start;
    padding: var(--space-2) var(--space-3);
  }

  .mobile-backdrop {
    display: block;
  }
}
</style>
