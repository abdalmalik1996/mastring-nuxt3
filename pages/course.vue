<template>
  <div
    class="flex flex-col items-center w-full h-full min-h-screen p-12 bg-blue-100"
  >
    <div class="mb-12 prose">
      <h1>
        <span class="font-medium">
          course :
          <span class="font-bold">Mastering Nuxt</span>
        </span>
      </h1>
    </div>
    <div class="flex flex-row justify-center flex-grow">
      <div
        class="prose mr-4 p-8 bg-white rounded-md min-w-[20ch] max-w-[30ch] flex flex-col"
      >
        <h3 class="font-bold">Chapters</h3>
        <div
          class="mt-4 mb-4 flex flex-col"
          v-for="chapter in chapters"
          :key="chapter.slug"
        >
          <h4 class="font-bold">{{ chapter.title }}</h4>
          <nuxtLink
            v-for="(lesson, index) in chapter.lessons"
            :key="lesson.slug"
            class="flex flex-row space-x-1 no-underline prose-sm font-normal py-1"
            :to="lesson.path"
            :class="{
              'text-blue-500': lesson.path === $route.fullPath,
              'text-gray-600': lesson.path !== $route.fullPath,
            }"
          >
            <span class="text-gray-500">{{ index + 1 }}</span>
            <span>{{ lesson.title }}</span>
          </nuxtLink>
        </div>
      </div>
      <div class="prose p-12 bg-white rounded-md w-[65ch]">
        <NuxtPage />
      </div>
    </div>
  </div>
</template>

<script setup>
const { chapters } = useCourse();
</script>

<style scoped></style>
