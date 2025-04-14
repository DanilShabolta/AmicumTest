<template>
  <button @click="toggleTheme" class="theme-button">
    {{ isDark ? "Светлая тема" : "Тёмная тема" }}
  </button>
</template>

<script>
export default {
  data() {
    return {
      isDark: false,
    };
  },
  mounted() {
    this.isDark = document.body.classList.contains("dark-theme");
  },
  methods: {
    toggleTheme() {
      this.isDark = !this.isDark;
      document.body.classList.toggle("dark-theme", this.isDark);
    },
  },
};
</script>

<style scoped>
.theme-button {
  padding: 6px 12px;
  background-color: var(--button-bg);
  color: var(--button-text);
  border: 1px solid var(--border-color);
  border-radius: 4px;
  cursor: pointer;
  margin-bottom: 20px;
}
</style>
