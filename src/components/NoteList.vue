<template>
  <div class="overflow-x-scroll">
    <div
      v-if="notes?.length === 0"
      class="flex justify-center items-center h-full"
    >
      <h1 class="text-sm tracking-widest">no notes yet. 😢</h1>
    </div>

    <div v-else class="mx-4 grid md:grid-cols-2 grid-cols-1 gap-4">
      <div
        class="md:max-w-md p-6 rounded-md shadow-lg flex items-center border md:h-[140px] h-[120px]"
        v-for="note in notes"
        :key="note.id"
      >
        <div class="flex-1 w-full h-full">
          <h5 class="mb-2 text-sm font-semibold tracking-tight">
            {{ note.title }}
          </h5>
          <p
            class="font-normal text-xs"
            style="overflow: hidden; text-overflow: ellipsis"
          >
            {{ note.body }}
          </p>
        </div>
        <div
          class="flex h-full text-sm text-red-600 cursor-pointer"
          @click="deleteNote(note.id)"
        >
          <i class="fa-regular fa-trash-can"></i>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { defineProps } from "vue";

const props = defineProps(["notes", "deleteNote"]);
</script>
