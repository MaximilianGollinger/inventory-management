<!--
  Reference view for `redesign-vue-saas` skill.

  Pattern that vue-expert applies when migrating each view's scoped style.
  Shows how the page-header, stats-grid, and card patterns consume tokens.

  This file is NOT loaded by the app. It exists as a stylistic target only.
-->
<template>
  <div class="view">
    <header class="page-header">
      <h2 class="page-title">{{ t('view.title') }}</h2>
      <p class="page-description">{{ t('view.description') }}</p>
    </header>

    <section class="stats-grid">
      <div v-for="stat in stats" :key="stat.id" class="stat-card">
        <span class="stat-label">{{ stat.label }}</span>
        <span class="stat-value">{{ stat.value }}</span>
        <span class="stat-delta" :class="stat.trend">{{ stat.delta }}</span>
      </div>
    </section>

    <section class="card">
      <header class="card-header">
        <h3 class="card-title">{{ t('view.cardTitle') }}</h3>
      </header>
      <div class="card-body">
        <!-- content -->
      </div>
    </section>
  </div>
</template>

<script>
export default {
  name: 'ExampleView'
}
</script>

<style scoped>
.view {
  display: flex;
  flex-direction: column;
  gap: var(--space-6);
}

.page-header {
  display: flex;
  flex-direction: column;
  gap: var(--space-1);
  margin-bottom: var(--space-4);
}

.page-title {
  margin: 0;
  font-size: var(--text-2xl);
  font-weight: var(--font-bold);
  letter-spacing: var(--tracking-tight);
  color: var(--color-text-primary);
}

.page-description {
  margin: 0;
  font-size: var(--text-sm);
  color: var(--color-text-secondary);
}

.stats-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(240px, 1fr));
  gap: var(--space-4);
}

.stat-card {
  display: flex;
  flex-direction: column;
  gap: var(--space-1);
  padding: var(--space-5);
  background: var(--color-surface);
  border: 1px solid var(--color-border);
  border-radius: var(--radius-md);
  transition: box-shadow var(--transition-base);
}

.stat-card:hover {
  box-shadow: var(--shadow-card-hover);
}

.stat-label {
  font-size: var(--text-xs);
  font-weight: var(--font-medium);
  color: var(--color-text-secondary);
  text-transform: uppercase;
  letter-spacing: var(--tracking-wide);
}

.stat-value {
  font-size: var(--text-3xl);
  font-weight: var(--font-bold);
  letter-spacing: var(--tracking-tight);
  color: var(--color-text-primary);
}

.stat-delta {
  font-size: var(--text-sm);
  font-weight: var(--font-medium);
}

.stat-delta.positive {
  color: var(--color-success);
}

.stat-delta.negative {
  color: var(--color-danger);
}

.stat-delta.neutral {
  color: var(--color-text-secondary);
}

.card {
  background: var(--color-surface);
  border: 1px solid var(--color-border);
  border-radius: var(--radius-md);
  padding: var(--space-5);
  transition: box-shadow var(--transition-base);
}

.card:hover {
  box-shadow: var(--shadow-card-hover);
}

.card-header {
  margin-bottom: var(--space-4);
  padding-bottom: var(--space-4);
  border-bottom: 1px solid var(--slate-100);
}

.card-title {
  margin: 0;
  font-size: var(--text-lg);
  font-weight: var(--font-semibold);
  color: var(--color-text-primary);
}

.card-body {
  font-size: var(--text-base);
  line-height: var(--leading-base);
  color: var(--color-text-primary);
}
</style>
