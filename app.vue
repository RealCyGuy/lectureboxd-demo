<template>
  <header class="p-5">
    <a
      href="/"
      class="text-white flex flex-row absolute justify-center items-center gap-3"
      ><img src="public/logo.png" alt="" class="w-10" />
      <span>lectureboxd</span></a
    >
  </header>
  <div class="text-white min-h-screen flex items-center justify-center text-lg">
    <NuxtPage />
  </div>
</template>

<style>
body {
  @apply bg-gray-900;
}
</style>
