<template>
  <v-footer height="40" app>
    <a
      v-for="item in items"
      :key="item.title"
      :href="item.href"
      :title="item.title"
      class="d-inline-block mx-2 social-link"
      rel="noopener noreferrer"
      target="_blank"
    >
      <v-icon :icon="item.icon" :size="24" />
    </a>
    <div class="text-caption text-disabled">Feito com Vuetify 3</div>

    <div
      class="text-caption text-disabled"
      style="position: absolute; right: 16px"
    >
      &copy; 2024-{{ new Date().getFullYear() }}
      <span class="d-none d-sm-inline-block"
        >Cotação de Criptomoedas em Tempo Real</span
      >
    </div>
  </v-footer>
</template>

<script setup>
const items = [
  {
    title: "GitHub",
    icon: `mdi-github`,
    href: "https://github.com/silvanat/cotacao-cripto",
  },
];
</script>

<style scoped lang="sass">
.social-link :deep(.v-icon)
  color: rgba(var(--v-theme-on-background), var(--v-disabled-opacity))
  text-decoration: none
  transition: .2s ease-in-out

  &:hover
    color: rgba(25, 118, 210, 1)
</style>
