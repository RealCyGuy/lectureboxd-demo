<template>
  <form
      class="flex items-stretch justify-center flex-col gap-7 w-full min-h-screen max-w-2xl px-7"
      @submit.prevent
  >
    <h1 class="text-xl text-center">Log Into Your Account</h1>
    <div class="flex flex-col gap-2">
      <label for="name">Name</label>
      <input
          type="text"
          id="name"
          class="border-2 border-white rounded px-5 py-2 bg-inherit outline-none placeholder:text-white/80"
          placeholder="John Doe"
      />
    </div>
    <div class="flex flex-col gap-2">
      <label for="email">Email</label>
      <input
          type="email"
          id="email"
          class="border-2 border-white rounded px-5 py-2 bg-inherit outline-none placeholder:text-white/80"
          placeholder="student@mail.mcgill.ca"
      />
    </div>
  </form>
</template>

<script setup>
import courses from "~/courses.txt?raw";

const courses_list = courses.split("\n");
const course = ref("");

const user_courses = reactive(new Set());
const autocomplete = computed(() => {
  return courses_list
      .filter((c) => c.toLowerCase().startsWith(course.value.toLowerCase()))
      .slice(0, 1000);
});
</script>
