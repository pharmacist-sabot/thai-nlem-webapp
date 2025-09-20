<template>
  <header class="header-container">
    <h1 class="title">NLEM Drug Search</h1>
    <p class="subtitle">เครื่องมือค้นหาข้อมูลยาจากบัญชียาหลักแห่งชาติ</p>
  </header>
</template>

<style scoped>
.header-container {
  text-align: center;
  margin-bottom: var(--spacing-8);
}
.title {
  font-size: 2.25rem;
  font-weight: 700;
  color: var(--color-text-primary);
  margin: 0;
}
.subtitle {
  font-size: 1.125rem;
  color: var(--color-text-secondary);
  margin-top: var(--spacing-2);
}

@media (max-width: 640px) {
  .title {
    font-size: 1.875rem;
  }
  .subtitle {
    font-size: 1rem;
  }
}
</style>