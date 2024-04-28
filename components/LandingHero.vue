<script setup lang="ts">
const people = [1, 2, 3, 4, 5, 6, 7];
const selectedPeople = ref(people[0]);

const percent = [32, 40, 48, 50, 60, 70, 80, 90, 100, 110, 120, 130, 140, 150, 160, 170, 180, 190, 200];
const selectedPerson = ref(100);

const 기준중위소득 = {
  2024: [2_228_445, 3_682_609, 4_714_657, 5_729, 913, 6_695, 735, 7_618_369, 8_514_994],
}

const 건강보험료율 = {
  2024: 7.09,
}

const 소득인정액 = ref(0);
const 건강보험료 = ref(0);
const myPercent = computed(() => {
  let 보수월액 = 0;
  if (selectedTab.value.id === '건강보험료') {
    보수월액 = 건강보험료.value * (100 / (건강보험료율[2024] / 2));
  } else {
    보수월액 = 소득인정액.value;
  }
  return Math.round(보수월액 / 기준중위소득[2024][selectedPeople.value - 1] * 1000) / 10;
})

const items = [
  {
    id: '소득인정액',
    label: '당신 가구의 월 소득인정액은?'
  },
  {
    id: '건강보험료',
    label: '당신 가구의 월 건강보험료는?'
  }
]
const selectedTab = ref(items[0]);

function onChange(index: number) {
  selectedTab.value = items[index];
}
</script>
<template>
  <div>
    <div class="mx-auto px-4 flex flex-col max-w-4xl mb-3">
      <div class="text-center relative z-[1]">
        <UBadge size="lg">2024</UBadge>
        <h1 class="text-5xl font-bold tracking-tight text-gray-900 dark:text-white mt-3">
          나의 중위소득은<br><span class="text-primary">몇 퍼센트</span>?
        </h1>
      </div>
    </div>
    <div>
      <div class="mb-6">
        <div>가족구성원</div>
        <USelectMenu v-model="selectedPeople" :options="people" />
      </div>
      <div class="mb-6">
        <UTabs :items="items" @change="onChange" />
        <UInput v-if="selectedTab.id === '소득인정액'" v-model="소득인정액" color="gray" variant="outline" type="number" />
        <template v-if="selectedTab.id === '건강보험료'">
          <div>건강보험료:{{ 건강보험료율[2024] }}%, 노인장기요양료 보험료 미포함, 직전3개월평균</div>
          <UInput v-model="건강보험료" color="gray" variant="outline" type="number" />
        </template>
      </div>
      <div class="w-full text-center ">
        <div>당신의 중위소득기준</div>
        <h1 class="text-5xl font-bold tracking-tight text-gray-900 dark:text-white mt-3">
          <span class="text-primary">{{ myPercent }}</span>%
        </h1>
      </div>
    </div>
    <div class="my-24">
      <div>역대 기준중위소득 확인 - 보건복지부</div>
      <div>https://www.mohw.go.kr/menu.es?mid=a10708010900</div>
      <div>역대 건강보험료율 확인 - 국민건강보험</div>
      <div>https://www.nhis.or.kr/nhis/minwon/retrieveJobCalcView.do</div>
    </div>
  </div>
</template>

<style lang="scss" scoped></style>
