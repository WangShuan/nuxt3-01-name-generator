<script setup lang="ts">
import { Gender, Popularity, Length, names, Name } from '@/data';

interface OptionsState {
  gender: Gender,
  popularity: Popularity,
  length: Length
}

const options = reactive<OptionsState>({
  gender: Gender.BOY,
  popularity: Popularity.UNIQUE,
  length: Length.ALL
})

const computedSelectedNames = () => {
  const filterNames = names.filter(name => name.gender === options.gender && name.popularity === options.popularity);
  if (options.length !== Length.ALL) {
    selectedNames.value = filterNames.filter(name => name.length === options.length).map(item => item.name)
  } else {
    selectedNames.value = filterNames.map(item => item.name)
  }
}

const optionsDatas = [
  {
    title: "1. 選擇寶寶性別",
    cate: 'gender',
    buttons: [
      Gender.BOY,
      Gender.UNISEX,
      Gender.GIRL
    ]
  },
  {
    title: "2. 選擇流行程度",
    cate: 'popularity',
    buttons: [
      Popularity.TRENDY,
      Popularity.UNIQUE
    ]
  },
  {
    title: "3. 選擇長度",
    cate: 'length',
    buttons: [
      Length.LONG,
      Length.ALL,
      Length.SHORT
    ]
  }
]

const selectedNames = ref<string[]>([])

const removeName = (name: string) => {
  const i = selectedNames.value.findIndex(item => item === name);
  selectedNames.value.splice(i, 1)
}
</script>

<template>
  <div class="container">
    <h1>
      寶寶英文名字產生器
    </h1>
    <p>
      選取您需要的條件，並點擊下方按鈕獲取結果。
    </p>
    <div class="options-container">
      <OptionContainer v-for="item in optionsDatas" :key="item.title" :optionData="item" :options="options" />
      <button @click="computedSelectedNames()" class="submit-btn">搜索姓名</button>
    </div>
    <ul v-if="selectedNames.length">
      <CardName @removeName="() => removeName(item)" :name="item" v-for="item in selectedNames" :key="item" />
    </ul>
  </div>
</template>

<style scoped>
* {
  box-sizing: border-box;
}

p {
  margin: 0;
}

ul {
  margin: 2rem auto 0;
  padding: 0;
  list-style: none;
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  align-items: center;
}

.container {
  font-family: Arial, Helvetica, sans-serif;
  color: #434343;
  text-align: center;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  padding: 20px;
  max-width: 600px;
  margin: auto;
}

h1 {
  font-size: 2rem;
  margin: 0 0 1rem;
  padding: 0;
}

.options-container {
  background-color: rgb(251, 220, 225);
  border-radius: 2rem;
  padding: 1rem;
  margin: 2rem auto 0;
  width: 100%;
}

.submit-btn {
  background-color: rgb(225, 94, 94);
  border: 1px solid rgb(225, 94, 94);
  padding: 0.35rem 0.75rem;
  font-size: 1rem;
  color: #fff;
  cursor: pointer;
  border-radius: 10px;
  width: 100%;
}
</style>
