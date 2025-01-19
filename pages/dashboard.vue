<template>
  <div class="flex items-center justify-center flex-col gap-5 p-10 w-full">
    <div
      v-for="user in users"
      class="w-full max-w-xl bg-gray-800/50 p-5 rounded-lg shadow-lg flex-col items-center gap-5"
    >
      <h2 class="text-lg">{{ user.name }}</h2>
      <p class="text-sm">
        <span :class="{ underline: user.year == data.year }"
          >U{{ user.year }}</span
        >
        {{ user.major }}
      </p>
      <div class="flex gap-5">
        <p v-for="course in user.courses" class="text-sm">{{ course }}</p>
      </div>
    </div>
  </div>
</template>

<script setup>
import courses from "~/courses.txt?raw";

const courses_list = courses.split("\n");
function formattedCourses(list) {
  let res = [];
  for (const course of list) {
    res.push(course.replace(/([a-zA-Z]+)(\d+)/, "$1 $2"));
  }
  return res;
}
const refactored_courses = formattedCourses(courses_list);
const users = [];
for (let i = 0; i < 10; i++) {
  let courses = new Set();
  let number_of_courses = Math.floor(Math.random() * 3) + 3;
  for (let j = 0; j < number_of_courses; j++) {
    courses.add(
      refactored_courses[Math.floor(Math.random() * refactored_courses.length)]
    );
  }
  users.push({
    name: "John Doe",
    major: "Computer Science",
    year: Math.floor(Math.random() * 5),
    courses: courses,
  });
}

const data = ref({});

onMounted(() => {
  data.value = localStorage.getItem("user");
  if (!data.value) {
    navigateTo("/");
  } else {
    data.value = JSON.parse(data.value);
  }
});
</script>
