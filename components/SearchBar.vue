<template>
  <div class="search-bar__wrap">
    <div class="search-bar">
      <UInput
        icon="i-heroicons-magnifying-glass-20-solid"
        size="sm"
        color="white"
        :trailing="false"
        placeholder="Search book here..."
        v-model="input"
      />
      <UButton color="primary" variant="solid" @click="$emit('search', input)"
        >Search</UButton
      >
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      input: "",
    };
  },
};
</script>
<style>
.search-bar__wrap {
  display: flex;
  align-items: center;
  justify-content: center;
}
.search-bar {
  display: flex;
  gap: 8px;
}
</style>
