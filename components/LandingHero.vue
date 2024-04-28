<script setup lang="ts">
const people = [1, 2, 3, 4, 5, 6, 7];
const selectedPeople = ref(people[0]);

const percent = [32, 40, 48, 50, 60, 70, 80, 90, 100, 110, 120, 130, 140, 150, 160, 170, 180, 190, 200];
const selectedPerson = ref(100);

const 기준중위소득 = {
  2024: [2_228_445, 3_682_609, 4_714_657, 5_729, 913, 6_695, 735, 7_618_369, 8_514_994],
}

const 내소득 = ref(0);

const myPercent = computed(() => {
  return Math.round(내소득.value / 기준중위소득[2024][selectedPeople.value - 1] * 1000) / 10
})

</script>
<template>
  <div class="py-24">
    <div class="mx-auto px-4 flex flex-col max-w-4xl">
      <div class="text-center relative z-[1]">
        <UBadge size="lg">2024</UBadge>
        <h1 class="text-5xl font-bold tracking-tight text-gray-900 dark:text-white mt-3">
          나의 중위소득은<br><span class="text-primary">몇 퍼센트</span>?
        </h1>
        <p class="mt-6 text-lg tracking-tight text-gray-600 dark:text-gray-300">
          <span> 100%에 해당하는 값들은 매년 7월에 보건복지부에서 공고 </span>
          <span> https://www.mohw.go.kr/menu.es?mid=a10708010900 </span>
        </p>
      </div>
    </div>
    <div>
      <div class="mb-3">
        <div>가족구성원</div>
        <USelectMenu v-model="selectedPeople" :options="people" />
      </div>
      <div class="mb-3">
        <div>당신의 소득?</div>
        <UInput v-model="내소득" color="gray" variant="outline" placeholder="소득 적어줘" type="number" />
      </div>
      <div class="w-full text-center">
        <div>당신은 중위소득기준</div>
        <h1 class="text-5xl font-bold tracking-tight text-gray-900 dark:text-white mt-3">
          <span class="text-primary">{{ myPercent }}</span>%
        </h1>

      </div>
    </div>
  </div>
</template>

<style lang="scss" scoped></style>
