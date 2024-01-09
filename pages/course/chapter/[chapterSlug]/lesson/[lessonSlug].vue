<template>
  <div>
    <p class="mt- uppercase font-bold text-slate-400 mb-1">
      Lesson {{ chapter.number }} - {{ lesson.number }}
    </p>
    <h2 class="my-0 font-bold">{{ lesson.title }}</h2>
    <div class="flex space-x-5 mt-2 mb-8">
      <NuxtLink v-if="lesson.sourceUrl" :to="lesson.sourceUrl"
        >Download Source Code</NuxtLink
      >
      <NuxtLink
        v-if="lesson.downloadUrl"
        :to="lesson.downloadUrl"
        class="font-normal text-md text-gray-500"
        >Download Video</NuxtLink
      >
    </div>
    <VideoPlayer v-if="lesson.videoId" :videoId="lesson.videoId" />
    <p>{{ lesson.text }}</p>
    <ClientOnly>
      <LessonComplateButton
        :model-value="isLessonComplate"
        @update:model-value="toogleComplate"
      />
    </ClientOnly>
  </div>
</template>

<script setup>
const course = useCourse();
const route = useRoute();

const chapter = computed(() => {
  return course.chapters.find(
    (chapter) => chapter.slug === route.params.chapterSlug
  );
});

const lesson = computed(() => {
  return chapter.value.lessons.find(
    (lesson) => lesson.slug === route.params.lessonSlug
  );
});

const title = computed(() => {
  return `${lesson.value.title} - ${course.title}`;
});

useHead({
  title: title,
});

const progress = useLocalStorage("progress", () => {
  return [];
});

const isLessonComplate = computed(() => {
  if (!progress.value[chapter.value.number - 1]) {
    return false;
  }

  if (!progress.value[chapter.value.number - 1][lesson.value.number - 1]) {
    return false;
  }
  return progress.value[chapter.value.number - 1][lesson.value.number - 1];
});

const toogleComplate = () => {
  if (!progress.value[chapter.value.number - 1]) {
    progress.value[chapter.value.number - 1] = [];
  }
  progress.value[chapter.value.number - 1][lesson.value.number - 1] =
    !isLessonComplate.value;
};
</script>
