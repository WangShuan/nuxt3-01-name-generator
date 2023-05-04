<template>
  <div class="option-container">
    <p>{{ optionData.title }}</p>
    <div class="option-buttons">
      <button v-for="option in optionData.buttons" :key="option" @click="clickOption(option)"
        :class="{ 'active': options[optionData.cate as 'gender' | 'popularity' | 'length'] === option }">
        {{ option }}
      </button>
    </div>
  </div>
</template>

<script setup lang="ts">
import { Gender, Popularity, Length } from '@/data';
interface OptionProps {
  optionData: {
    title: string;
    cate: string;
    buttons: Gender[] | Popularity[] | Length[];
  }
  options: {
    gender: Gender;
    popularity: Popularity;
    length: Length;
  }
}
const props = defineProps<OptionProps>()

const clickOption = (value: Gender | Popularity | Length) => {
  if (props.optionData.cate === 'gender') {
    props.options[props.optionData.cate] = value as Gender;
  } else if ((props.optionData.cate === 'popularity')) {
    props.options[props.optionData.cate] = value as Popularity;
  } else if ((props.optionData.cate === 'length')) {
    props.options[props.optionData.cate] = value as Length;
  }
}
</script>


<style scoped>
.option-container {
  margin-bottom: 2rem;
}

.option-buttons {
  margin: 1rem auto;
}

.option-buttons>button {
  background-color: #fff;
  border: 1px solid rgb(225, 94, 94);
  padding: 0.35rem 0.75rem;
  font-size: 1rem;
  color: #434343;
  cursor: pointer;
  border-radius: 10px;
  margin: 5px;
}

.option-buttons>button.active {
  background-color: rgb(225, 94, 94);
  color: #fff;
}
</style>