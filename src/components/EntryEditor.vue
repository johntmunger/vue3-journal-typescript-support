<script lang="ts" setup>
import EmojiField from "@/components/EmojiField.vue";
import ArrowCircleRight from "@/assets/icons/arrow-circle-right.svg";
import type Emoji from "@/types/Emoji";
import { ref, computed, onMounted, inject } from "vue";
import Entry from "@/types/Entry";

// injection keys versus props
import { userInjectionKey } from "@/injectionKeys";
const user = inject(userInjectionKey);

// data
const body = ref("");
const emoji = ref<Emoji | null>(null);
const charCount = computed(() => body.value.length);
const maxChars = 280;

//events
const emit = defineEmits<{
  (e: "@create", entry: Entry): void;
}>();

// template refs
const textarea = ref<HTMLTextAreaElement | null>(null);
onMounted(() => {
  textarea.value?.focus();
});

// methods
const handleTextInput = (e: Event) => {
  const textarea = e.target as HTMLTextAreaElement;
  if (textarea.value.length <= maxChars) {
    body.value = textarea.value;
  } else {
    body.value = textarea.value = textarea.value.substring(0, maxChars);
  }
};

const handleSubmit = () => {
  const newEntry: Entry = {
    id: Date.now(),
    body: body.value,
    emoji: emoji.value,
    createdAt: new Date(),
    userId: 1, // assuming a static user for this example
  };
  emit("@create", newEntry);
  // reset form
  body.value = "";
  emoji.value = null;
};
</script>

<template>
  <form id="entry-form" class="entry-form" @submit.prevent="handleSubmit()">
    <textarea
      ref="textarea"
      :value="body"
      @keyup="handleTextInput"
      :placeholder="`New Journal Entry for ${user?.username || 'anonymous user'}`"
    >
    </textarea>
    <EmojiField v-model="emoji" />
    <div class="entry-form-footer">
      <span>{{ charCount }} / {{ maxChars }}</span>
      <button>Remember <ArrowCircleRight width="20" /></button>
    </div>
  </form>
</template>
