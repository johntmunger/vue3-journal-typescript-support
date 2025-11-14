<script lang="ts" setup>
import DateDisplay from "./DateDisplay.vue";
import UseEmojis from "@/composables/UseEmojis";
import Entry from "@/types/Entry";

// injection keys versus props
import { inject } from "vue";
import { userInjectionKey } from "@/injectionKeys";
const user = inject(userInjectionKey);

const { findEmoji } = UseEmojis();
defineProps<{
  entry: Entry;
}>();
</script>
<template>
  <div class="entry-card">
    <div class="entry-card-body">
      <component width="75" :is="findEmoji(entry.emoji)"></component>
      <div class="entry-text">{{ entry.body }}</div>
    </div>
    <div class="entry-footer">
      <DateDisplay :date="entry.createdAt" class="mr-2" />
      |
      <span class="ml-2">{{ user?.username || "anyonymous user" }}</span>
    </div>
  </div>
</template>
