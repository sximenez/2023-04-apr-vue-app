<script setup>
import { ref } from "vue";

const sourceLink = ref("https://vuejs.org/examples/#crud");
</script>

<template>
  <div class="nav border-b">
    <div class="nav__left">
      <img src="../assets/vue.svg" alt="Vue logo" />
      <h1 class="nav__left--h1">Vue CRUD</h1>
    </div>
    <a :href="sourceLink" target="_blank" class="btn">Source</a>
  </div>
</template>

<style scoped lang="scss">
.nav {
  height: 100px;
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  justify-content: space-between;
  position: sticky;
  top: 0;

  padding: 0 1.5rem;

  @media (min-width: 1280px) {
    padding: 0 16rem;
  }

  @media (min-width: 1024px) {
    margin-right: 1rem;
  }

  background-color: white;

  &__left {
    display: flex;
    align-items: center;
    gap: 0.75rem;

    text-align: center;

    @media (min-width: 640px) {
      text-align: start;
    }

    &--h1 {
      width: 100%;

      white-space: nowrap;
    }
  }
}

.btn {
  border: 1px solid lightgray;
  border-radius: 0.5rem;
  padding: 0.5rem 1rem;

  text-align: center;

  &:hover {
    background-color: #d3d3d350;
    border-color: #d3d3d350;
  }
}
</style>
