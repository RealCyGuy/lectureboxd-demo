<template>
  <form
    class="flex items-stretch justify-center flex-col gap-7 w-full min-h-screen max-w-2xl px-7"
    @submit.prevent
  >
    <h1 class="text-xl text-center">Set up your account</h1>
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
    <div class="flex flex-col gap-2">
      <label for="major">Major</label>
      <input
        type="text"
        id="major"
        class="border-2 border-white rounded px-5 py-2 bg-inherit outline-none placeholder:text-white/80"
        placeholder="Computer Science"
      />
    </div>
    <div class="flex flex-col gap-2">
      <label for="year">Year</label>
      <select
        id="year"
        class="border-2 border-white rounded px-5 py-2 bg-inherit outline-none placeholder:text-white/80"
        placeholder="student@mail.mcgill.ca"
      >
        <option v-for="i in Array(10).keys()" :value="i">U{{ i }}</option>
      </select>
    </div>
    <div class="flex flex-col gap-2">
      <label for="courses">What courses are you taking this semester?</label>
      <ul class="flex flex-wrap gap-2">
        <li v-for="course in user_courses">
          <button
            class="bg-gray-500 hover:bg-gray-600 duration-100 px-5 py-2 rounded-full text-base"
            @click="user_courses.delete(course)"
          >
            {{ course }}
          </button>
        </li>
      </ul>
      <input
        type="text"
        id="courses"
        class="border-2 border-white rounded px-5 py-2 bg-inherit outline-none placeholder:text-white/80"
        v-model="course"
        placeholder="Search courses..."
      />
      <div class="flex flex-col gap-5 justify-between">
        <ul class="overflow-scroll max-h-40 flex flex-wrap gap-2">
          <li v-for="course in autocomplete" :key="course">
            <button
              class="bg-gray-500 hover:bg-gray-600 duration-100 px-3 py-2"
              @click="user_courses.add(course)"
            >
              {{ course }}
            </button>
          </li>
        </ul>
      </div>
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
