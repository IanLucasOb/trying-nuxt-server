<template>
  <div>
    <h1>TODO SOMETHING HERE</h1>

    <h4>{{ supabaseUser?.email }}</h4>
  </div>
</template>

<script setup lang="ts">
const supabaseUser = useSupabaseUser();
</script>
