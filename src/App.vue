<template>
  <div
    class="flex flex-col justify-between mx-auto md:w-2/4 w-full bg-gray-100 min-h-screen shadow-lg"
  >
    <Navbar />
    <Input />
    <NoteList />
    <Footer />
  </div>
</template>

<script setup>
import Navbar from "./components/Navbar.vue";
import Input from "./components/Input.vue";
import NoteList from "./components/NoteList.vue";
import Footer from "./components/Footer.vue";
</script>
