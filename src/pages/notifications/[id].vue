<template>
    <NotificationPage :page="page" />
</template>

<script setup lang="ts">
import NotificationPage from '@/components/NotificationPage.vue';

const route = useRoute();
const page = ref(Number((route.params as { id: string }).id) || 0);
</script>

<route lang="yaml">
meta:
  layout: appbar
</route>