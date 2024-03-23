<script setup lang="ts">
import type { Task } from "~/types";
import { nanoid } from "nanoid";

const emit = defineEmits<{ (e: "add", payload: Task): void }>();

const focused = ref(false);
const title = ref("");

function createTask(e: Event) {
  if (title.value.trim()) {
    e.preventDefault();
    emit("add", {
      title: title.value.trim(),
      createdAt: new Date(),
      id: nanoid(),
    } as Task);
  }
  title.value = "";
}
</script>

<template>
  <div>
    <textarea
      v-model="title"
      @keydown.tab="createTask"
      @keydown.enter="createTask"
      class="focus:bg-white bg-transparent focus:shadow focus:p-2 resize-none rounded w-full border-none cursor-pointer"
      :class="{
        'h-7': !focused,
        'h-20': focused,
      }"
      style="outline: none !important"
      @focus="focused = true"
      @blur="focused = false"
      :placeholder="!focused ? '+ Add A Card' : 'Enter a title for this card'"
    ></textarea>
  </div>
</template>
