<template>
  <div class="md:max-w-md max-w-xs w-full mx-auto my-8">
    <h1 class="capitalize font-medium tracking-wider md:text-base text-md">
      create note
    </h1>

    <form
      class="flex flex-col my-4 space-y-4 border shadow-lg rounded-md p-4 text-sm capitalize"
      @submit.prevent="createNote"
    >
      <div class="flex flex-col space-y-2">
        <label for="title" class="font-medium tracking-wider">title</label>
        <input
          type="text"
          v-model="title"
          @input="limitTitleChars"
          id="title"
          placeholder="add title"
          class="px-2 py-1 rounded-md text-sm placeholder:text-xs placeholder:capitalize"
        />
        <div class="text-[10px] text-right">
          max characters 15. remaining
          <span
            :class="
              remainingTitleChars === 0
                ? 'text-red-600 font-medium'
                : 'text-inherit'
            "
          >
            {{ remainingTitleChars }}
          </span>
        </div>
      </div>

      <div class="flex flex-col space-y-2">
        <label for="body" class="font-medium tracking-wider">note</label>
        <textarea
          placeholder="add note..."
          v-model="body"
          @input="limitBodyChars"
          id="body"
          rows="5"
          class="resize-none px-2 py-1 rounded-md text-sm placeholder:text-xs placeholder:capitalize"
        />
        <div class="text-[10px] text-right">
          max characters 50. remaining
          <span
            :class="
              remainingBodyChars === 0
                ? 'text-red-600 font-medium'
                : 'text-inherit'
            "
          >
            {{ remainingBodyChars }}
          </span>
        </div>
      </div>

      <input
        type="submit"
        value="create"
        :disabled="title === '' || body === ''"
        class="w-full text-sm bg-blue-600 hover:bg-blue-700 text-white capitalize py-1 rounded-md cursor-pointer disabled:bg-slate-400 disabled:cursor-not-allowed"
      />
    </form>
  </div>

  <h1 class="mx-6 my-2 md:text-base text-md capitalize font-medium">
    my notes
  </h1>
  <note-list :notes="notes" :deleteNote="deleteNote" class="h-[160px]" />
</template>

<script setup>
import { computed, ref, watch, onMounted } from "vue";
import NoteList from "./NoteList.vue";

const title = ref("");
const body = ref("");
const notes = ref([]);

const limitTitleChars = () => {
  title.value = title.value.slice(0, 15);
};

const limitBodyChars = () => {
  body.value = body.value.slice(0, 50);
};

const remainingTitleChars = computed(() => {
  return title.value.length > 15 ? 0 : 15 - title.value.length;
});

const remainingBodyChars = computed(() => {
  return body.value.length > 50 ? 0 : 50 - body.value.length;
});

const createNote = () => {
  const newNote = { id: +new Date(), title: title.value, body: body.value };
  notes.value.push(newNote);
  title.value = "";
  body.value = "";

  localStorage.setItem("notes", JSON.stringify(notes.value));
};

const deleteNote = (id) => {
  notes.value = notes.value.filter((note) => note.id !== id);
};

watch(notes, (newVal) => {
  localStorage.setItem("notes", JSON.stringify(newVal));
});

onMounted(() => {
  const notesFromLocalStorage = JSON.parse(localStorage.getItem("notes"));
  if (notesFromLocalStorage) {
    notes.value = notesFromLocalStorage;
  }
});
</script>
