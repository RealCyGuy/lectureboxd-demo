<template>
  <div class="flex items-center justify-center flex-col gap-5 p-10 w-screen">
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
        <p
            v-for="course in user.courses"
            class="text-sm"
            :class="{ underline: data.courses.includes(course) }"
            v-if="data.courses"
        >
          {{ course }}
        </p>
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
const users = ref([]);

const data = ref({});

onMounted(() => {
  data.value = localStorage.getItem("user");
  if (!data.value) {
    navigateTo("/");
  } else {
    data.value = JSON.parse(data.value);
    console.log(data.value);
  }
  for (let i = 0; i < 10; i++) {
    let courses = new Set();
    let number_of_courses = Math.floor(Math.random() * 3) + 3;
    let number_of_same_courses = Math.floor(Math.random() * 2) + 2;
    for (let j = 0; j < number_of_same_courses; j++) {
      courses.add(
          data.value.courses[
              Math.floor(Math.random() * data.value.courses.length)
              ]
      );
    }
    for (let d = 0; d < number_of_courses - courses.size; d++) {
      courses.add(
          refactored_courses[
              Math.floor(Math.random() * refactored_courses.length)
              ]
      );
    }
    const majors = [
      "Computer Science",
      "Mathematics",
      "Physics",
      "Chemistry",
      "Biology",
      "Soil Science",
      "Mechanical Engineering",
      "Software Engineering",
      "English",
      "French",
      "German Studies",
      "Psychology",
      "Finance",
    ];
    const first_names = ["Cyrus", "Walid", "Emily", "Isabel", "James", "Michael", "Jack", "Tyler", "Lawrence", "Miley", "Julia", "Kanye", "William", "Zachary", "Elon", "Smith"];
    const last_names = ["Doe", "Smith", "Cyrus", "Outlaw", "Solace", "Bardot", "West", "Bieber", "Cuban", "Trump", "McGill", "Came", "Jamie", "Henderson", "Z", "Devpost", "Name", "Poli"];
    users.value.push({
      name:
          first_names[Math.floor(Math.random() * first_names.length)] +
          " " +
          last_names[Math.floor(Math.random() * last_names.length)],
      major: majors[Math.floor(Math.random() * majors.length)],
      year: Math.floor(Math.random() * 5),
      courses: courses,
    });
  }
});
</script>
