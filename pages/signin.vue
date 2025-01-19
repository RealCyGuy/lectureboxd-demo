<template>
  <form
    class="pt-10 flex items-stretch justify-start flex-col gap-7 w-full min-h-screen max-w-2xl px-7"
    @submit.prevent="submit"
  >
    <h1 class="text-xl text-center">Set up your account</h1>
    <div class="flex flex-col gap-2">
      <label for="name">Name</label>
      <input
        type="text"
        id="name"
        class="border-2 border-white rounded px-5 py-2 bg-inherit outline-none placeholder:text-white/60"
        required
        placeholder="John Doe"
      />
    </div>
    <div class="flex flex-col gap-2">
      <label for="email">Email</label>
      <input
        type="email"
        id="email"
        class="border-2 border-white rounded px-5 py-2 bg-inherit outline-none placeholder:text-white/60"
        required
        placeholder="student@mail.mcgill.ca"
      />
    </div>
    <div class="flex flex-col gap-2">
      <label for="major">Major</label>
      <input
        type="text"
        id="major"
        class="border-2 border-white rounded px-5 py-2 bg-inherit outline-none placeholder:text-white/60"
        required
        placeholder="Computer Science"
      />
    </div>
    <div class="flex flex-col gap-2">
      <label for="year">Year</label>
      <select
        id="year"
        class="border-2 border-white rounded px-5 py-2 bg-inherit outline-none placeholder:text-white/60"
        required
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
            class="bg-gray-500 hover:bg-gray-600 duration-100 px-5 py-2 rounded-full text-sm"
            @click.prevent="user_courses.delete(course)"
            type="button"
          >
            {{ course }}
          </button>
        </li>
      </ul>
      <input
        type="text"
        id="courses"
        class="border-2 border-white rounded px-5 py-2 bg-inherit outline-none placeholder:text-white/60"
        v-model="course"
        placeholder="Search courses..."
        autocomplete="off"
      />
      <div class="flex flex-col gap-5 justify-between relative">
        <ul
          class="overflow-scroll h-40 flex flex-wrap gap-2 absolute w-full bg-gray-700 rounded p-2"
          v-if="course"
        >
          <li v-for="course in autocomplete" :key="course">
            <button
              class="bg-gray-500 hover:bg-gray-600 duration-100 px-3 py-2"
              @click="user_courses.add(course)"
              type="button"
            >
              {{ course }}
            </button>
          </li>
        </ul>
      </div>
    </div>
    <div class="flex justify-center">
      <button
        type="submit"
        class="border-2 border-white px-7 py-3 rounded-full"
      >
        Submit
      </button>
    </div>
  </form>
</template>

<script setup>
import courses from "~/courses.txt?raw";

const courses_list = courses.split("\n");
const course = ref("");
const refactored_courses = formattedCourses(courses_list);

const user_courses = reactive(new Set());
const autocomplete = computed(() => {
  return refactored_courses
    .filter((c) =>
      normaliseCourses(c).startsWith(normaliseCourses(course.value))
    )
    .slice(0, 1000);
});

function formattedCourses(list) {
  let res = [];
  for (const course of list) {
    res.push(course.replace(/([a-zA-Z]+)(\d+)/, "$1 $2"));
  }
  return res;
}

function normaliseCourses(course){
  return course.replaceAll(/([^a-zA-Z0-9]+)/g, '').toLowerCase();
}

async function submit() {
  const data = {
    name: document.getElementById("name").value,
    email: document.getElementById("email").value,
    major: document.getElementById("major").value,
    year: document.getElementById("year").value,
    courses: Array.from(user_courses),
  };
  localStorage.setItem("user", JSON.stringify(data));
  navigateTo("/dashboard");
}
</script>
