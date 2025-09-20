<script setup lang="ts">
interface Drug {
  generic_name: string;
  dosage_forms: string[];
  ed_level: string | null;
}

defineProps<{
  drug: Drug
}>()
</script>

<template>
  <article class="drug-card">
    <div class="card-header">
      <h2 class="drug-name">{{ drug.generic_name }}</h2>
      <span v-if="drug.ed_level" class="ed-badge">
        บัญชี {{ drug.ed_level }}
      </span>
    </div>
    <div class="card-body">
      <h3 class="dosage-title">รูปแบบยา (Dosage Forms):</h3>
      <p class="dosage-text">
        {{ drug.dosage_forms.join(', ') || 'ไม่มีข้อมูล' }}
      </p>
    </div>
  </article>
</template>

<style scoped>
.drug-card {
  background-color: var(--color-card-bg);
  border: 1px solid var(--color-border);
  border-radius: var(--border-radius);
  padding: var(--spacing-4) var(--spacing-6);
  box-shadow: var(--shadow-sm);
  transition: transform 0.2s, box-shadow 0.2s;
}
.drug-card:hover {
  transform: translateY(-2px);
  box-shadow: var(--shadow-md);
}
.card-header {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  align-items: flex-start;
  gap: var(--spacing-3);
  margin-bottom: var(--spacing-3);
}
.drug-name {
  font-size: 1.25rem;
  font-weight: 600;
  margin: 0;
}
.ed-badge {
  background-color: rgba(79, 70, 229, 0.1);
  color: var(--color-primary);
  padding: var(--spacing-1) var(--spacing-3);
  border-radius: 9999px;
  font-size: 0.875rem;
  font-weight: 500;
  flex-shrink: 0;
}
.dosage-title {
  font-size: 0.875rem;
  font-weight: 500;
  color: var(--color-text-secondary);
  margin: 0 0 var(--spacing-2) 0;
}
.dosage-text {
  font-size: 1rem;
  color: var(--color-text-primary);
  margin: 0;
}
</style>