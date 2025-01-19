<template>
  <form
    class="flex items-stretch justify-center flex-col gap-7 w-full min-h-screen max-w-2xl px-7"
    @submit.prevent="submit"
  >
    <h1 class="text-xl text-center">Log Into Your Account</h1>
    <div class="flex flex-col gap-2">
      <label for="name">Name</label>
      <input
        type="text"
        id="name"
        class="border-2 border-white rounded px-5 py-2 bg-inherit outline-none placeholder:text-white/80"
        placeholder="John Doe"
        required
      />
    </div>
    <div class="flex flex-col gap-2">
      <label for="email">Email</label>
      <input
        type="email"
        id="email"
        class="border-2 border-white rounded px-5 py-2 bg-inherit outline-none placeholder:text-white/80"
        placeholder="student@mail.mcgill.ca"
        required
      />
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
function submit() {
  const data = localStorage.getItem("user");
  if (!data) {
    alert("User not found");
  } else {
    navigateTo("/dashboard");
  }
}
</script>
