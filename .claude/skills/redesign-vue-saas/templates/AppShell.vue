<!--
  Template for the new client/src/App.vue structure.

  CRITICAL — vue-expert, when applying this:
  - Copy the ENTIRE existing <script> block from client/src/App.vue verbatim.
    Only ADD the AppSidebar import and component registration. Do NOT alter
    refs, computed, watch, lifecycle hooks, modal state, or any setup() body.
  - In the <template>, REPLACE the top-nav <header> block with <AppSidebar />.
    Preserve EVERY modal mount and the FilterBar mount, repositioned inside
    .app-main as shown.
  - Adopt the <style> block below; merge any view-spanning global selectors
    from the existing App.vue (if they don't fit a scoped style block in a
    view file).
-->
<template>
  <div class="app-shell">
    <AppSidebar />

    <div class="app-main">
      <FilterBar />
      <main class="app-content">
        <router-view />
      </main>
    </div>

    <!--
      Modal mounts: COPY VERBATIM from the existing client/src/App.vue.
      Examples (replace with actual mounts found in the existing file):

      <ProfileDetailsModal v-if="profileOpen" @close="profileOpen = false" />
      <TasksModal v-if="tasksOpen" :tasks="tasks" @close="tasksOpen = false" />
      <InventoryDetailModal v-if="...." />
      <ProductDetailModal v-if="...." />
      <CostDetailModal v-if="...." />
      <BacklogDetailModal v-if="...." />
    -->
  </div>
</template>

<script>
/*
  VUE-EXPERT INSTRUCTIONS:

  1. Open the existing client/src/App.vue.
  2. Copy the entire <script> block (export default { ... }) into this file.
  3. Add the import:    import AppSidebar from './components/AppSidebar.vue'
  4. Add AppSidebar to the `components: { ... }` registration block.
  5. Leave EVERY other field (props, setup, computed, watch, lifecycle) untouched.
  6. The placeholder below is a stub — replace with the merged real version.
*/

import AppSidebar from './components/AppSidebar.vue'
// Preserve existing imports here:
// import FilterBar from './components/FilterBar.vue'
// import ProfileDetailsModal from './components/ProfileDetailsModal.vue'
// import TasksModal from './components/TasksModal.vue'
// ... etc.

export default {
  name: 'App',
  components: {
    AppSidebar
    // Preserve existing component registrations
  },
  setup() {
    // Preserve the entire existing setup() body verbatim.
    // This stub exists only as a placeholder.
    return {}
  }
}
</script>

<style>
/* ============================================================
 * Global resets and base
 * ============================================================ */
* {
  box-sizing: border-box;
}

html, body {
  margin: 0;
  padding: 0;
  font-family: var(--font-sans);
  color: var(--color-text-primary);
  background: var(--color-bg);
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

#app {
  min-height: 100vh;
}

/* ============================================================
 * App shell layout
 * ============================================================ */
.app-shell {
  display: grid;
  grid-template-columns: var(--sidebar-width) 1fr;
  min-height: 100vh;
  background: var(--color-bg);
  transition: grid-template-columns var(--transition-base);
}

.app-shell:has(.sidebar.collapsed) {
  grid-template-columns: var(--sidebar-width-collapsed) 1fr;
}

.app-main {
  display: flex;
  flex-direction: column;
  min-width: 0;
}

.app-content {
  flex: 1;
  padding: var(--space-6) var(--space-8);
  max-width: 1600px;
  width: 100%;
}

/* ============================================================
 * Mobile (<768px) — sidebar becomes off-canvas drawer
 * ============================================================ */
@media (max-width: 768px) {
  .app-shell,
  .app-shell:has(.sidebar.collapsed) {
    grid-template-columns: 1fr;
  }

  .app-content {
    padding: var(--space-4);
  }
}
</style>
