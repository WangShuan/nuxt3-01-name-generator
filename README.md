# 01-name-generator

## 啟動 Nuxt 應用程序

執行命令：

```shell
npm install
```

安裝所有依賴項目，此時會發現專案目錄中生成了 `node_modules` 資料夾

確認您的專案已成功安裝好所有依賴後，即可執行命令：

```shell
npm run dev
```

啟動 Nuxt 應用程序
此時根據提示可通過 `http://localhost:3000/` 開啟畫面預覽

## 主要程式碼 app.vue

在專案目錄中可以看到 `app.vue` 檔案，
該檔案為整個專案的**入口畫面**，
在 `app.vue` 檔案中默認會有初始畫面用的元件 `<NuxtWelcome />`
將該元件移除後即可開始撰寫自己的程式碼

程式碼如同 `Vue.js` 一樣，區分為三大部分：

1. **template** - 撰寫 `HTML` 的部分
2. **script** - 撰寫 `JavaScript` 的部分
3. **style** - 撰寫 `CSS` 的部分

---

首先於 `HTML` 的部分，
設置一個 `h1` 設置標題與一個 `p` 段落說明使用方式：

```htmlembedded
<template>
  <h1>
    寶寶英文名字產生器
  </h1>
  <p>
    選取您需要的條件，並點擊下方按鈕獲取結果。
  </p>
</template>
```

接著設置一個 `div` 裡面放置 `p` 段落說明選擇的內容與一組 `button` 按鈕顯示所有選項
重複拷貝三次，分別用於性別、流行程度以及名字長度：

```htmlembedded
<template>
  ...
  <div class="option-container">
    <p>1. 選擇寶寶性別</p>
    <div class="option-buttons">
      <button>男性</button>
      <button>通用</button>
      <button>女性</button>
    </div>
  </div>
  <div class="option-container">
    <p>2. 選擇流行程度</p>
    <div class="option-buttons">
      <button>唯一的</button>
      <button>常見的</button>
    </div>
  </div>
  <div class="option-container">
    <p>3. 選擇長度</p>
    <div class="option-buttons">
      <button>全部</button>
      <button>長的</button>
      <button>短的</button>
    </div>
  </div>
</template>
```

最後新增一個 `button` 按鈕用來執行搜尋與一個列表用來顯示結果：

```htmlembedded
<template>
  ...
  <button class="submit-btn">搜索姓名</button>
  <ul>
    <li>name</li>
  </ul>
</template>
```

---

接著開始撰寫 JS 的部分，

在姓名產生器這個專案中，將使用 `TypeScript` 來撰寫 JS
第一步先建立 `script` 標籤，並設置為 `TypeScript` 語言：

```htmlembedded
<script setup lang="ts">
</script>
```

> 於 script 標籤中**新增屬性 setup 表示要使用 `Composition API` 寫法**
> 由於 Nuxt3 本身就支援 TS 所以可以直接透過 lang 設置語言類型(不設置 lang 默認就是 JS)

接著使用 TS 提供的 `enum` 羅列出性別、流行程度、長度的所有選項：

```typescript
// 性別 Gender: 男性 Boy、女性 Girl、通用 Unisex
enum Gender {
  GIRL = "Girl",
  BOY = "Boy",
  UNISEX = "Unisex",
}
// 流行程度 Popularity: 唯一的 Unique、常見的 Trendy
enum Popularity {
  UNIQUE = "Unique",
  TRENDY = "Trendy",
}
// 長度 Length: 全部 All、長的 Long、短的 Short
enum Length {
  ALL = "All",
  LONG = "Long",
  SHORT = "Short",
}
```

> 這邊使用 enum 羅列出選項後
> 性別、流行程度、長度的選項值就**被限制在 enum 中列舉出的內容**
> 比如 Gender 中有 GIRL、BOY、UNISEX 三個選項
> Gender **從此就只能是三個選項之中的其中一個，不能是這三個選項以外的內容**

再來使用 TS 提供的 `interface` 初始化物件的類型：

```typescript
// 先定義好選項有哪些、分別是什麼值
interface OptionsState {
  // 使用 interface 聲明 OptionsState 物件的類型
  gender: Gender; // gender 選項值只能為 eunm Gender 中的選項
  popularity: Popularity; // popularity 選項值只能為 eunm Popularity 中的選項
  length: Length; // length 選項值只能為 eunm Length 中的選項
}
```

最後即可設置我們的初始值：

```typescript
const options = reactive<OptionsState>({
  // 使用 <> 設置物件類型為 OptionsState
  gender: Gender.BOY, // 設定性別的預設值為 enum 中的 'Boy'
  popularity: Popularity.UNIQUE, // 設定流行程度的預設值為 enum 中的 'Unique'
  length: Length.ALL, // 設定長度的預設值為 enum 中的 'All'
});
```

## 將重複內容進行元件化 OptionContainer.vue

在 Nuxt3 中可以通過於項目根目錄中新增資料夾 `components` 來存放元件
且**資料夾名稱必須叫做 components**
主要原因是 Nuxt3 可以利用資料夾名稱實現**自動引入**
而不需要在檔案中多寫 `import` 程式碼

元件的名稱與 `Vue.js` 一樣，必須是**大駝峰表示法**
使用元件時，也與 `Vue.js` 一樣，可以用 `<FooBar />` 或 `<foo-bar />`

另外當有元件重複使用同樣開頭名稱時(EX: `CardTitle.vue` 與 `CardButton.vue`)
可以直接於 `components` 資料夾中再新增一個資料夾 `Card`
並在 `Card` 資料夾中新增檔案 `Title.vue` 與 `Button.vue` 以減少撰寫的程式碼量
(也可以在 `Card` 資料夾中新增檔案 `CardTitle.vue` 與 `CardButton.vue`，Nuxt 會自動幫您刪減掉重複的名稱內容)
**使用元件時則一樣要用 `<CardTitle />` 與 `<CardButton />` 標籤，不得省略 “Card”**

> Card 資料夾名稱建議同元件一樣使用大駝峰表示法命名，但用全小寫也欸通～！

---

在該專案中可以將選擇性別、流行程度、長度的區塊建立成元件：

1. 於項目根目錄中新增資料夾 `components`
2. 於 `components` 資料夾中新增檔案 `OptionContainer.vue`
3. 將 `template` 標籤中的 `.option-container` 區塊剪下放到 `OptionContainer.vue` 中

```htmlembedded
<!-- OptionContainer.vue 中 -->

<template>
  <div class="option-container">
    <p>1. 選擇寶寶性別</p>
    <div class="option-buttons">
      <button>男性</button>
      <button>通用</button>
      <button>女性</button>
    </div>
  </div>
  <div class="option-container">
    <p>2. 選擇流行程度</p>
    <div class="option-buttons">
      <button>唯一的</button>
      <button>常見的</button>
    </div>
  </div>
  <div class="option-container">
    <p>3. 選擇長度</p>
    <div class="option-buttons">
      <button>全部</button>
      <button>長的</button>
      <button>短的</button>
    </div>
  </div>
</template>
```

接著回到 `app.vue` 中聲明 `OptionContainer.vue` 需要用到的資料：

```typescript
// app.vue 的 script 標籤中

const optionsDatas = [
  // 新增 optionsDatas 為一個陣列
  {
    title: "1. 選擇寶寶性別", // 將 p 段落中的內容設為 title
    cate: "gender", // 設定分類為性別
    buttons: [
      // 新增性別的所有按鈕內容
      Gender.BOY, // 依序傳入所有 enum
      Gender.UNISEX,
      Gender.GIRL,
    ],
  },
  {
    title: "2. 選擇流行程度", // 將 p 段落中的內容設為 title
    cate: "popularity", // 設定分類為流行程度
    buttons: [
      // 新增流行程度的所有按鈕內容
      Popularity.TRENDY, // 依序傳入所有 enum
      Popularity.UNIQUE,
    ],
  },
  {
    title: "3. 選擇長度", // 將 p 段落中的內容設為 title
    cate: "length", // 設定分類為長度 length
    buttons: [
      // 新增長度的所有按鈕內容
      Length.LONG, // 依序傳入所有 enum
      Length.ALL,
      Length.SHORT,
    ],
  },
];
```

然後將 `Gender`、`Popularity`、`Length` 這三個 `enum` 取出，
改放在項目根目錄中新增的檔案 `data.ts` 裡面，
以便於在需要的地方可以通過 `import` 隨時引入使用：

```typescript
// data.ts 中

export enum Gender {
  GIRL = "Girl",
  BOY = "Boy",
  UNISEX = "Unisex",
}
export enum Popularity {
  UNIQUE = "Unique",
  TRENDY = "Trendy",
}
export enum Length {
  ALL = "All",
  LONG = "Long",
  SHORT = "Short",
}
```

接著把原來放置 `.option-container` 區塊的位置改為放置元件
並藉由 `v-for` 遍歷 `optionsDatas` 將傳遞需要的數據傳進元件中：

```htmlembedded
<!-- app.vue 中 -->

<template>
  <div class="container">
    <h1>
      寶寶英文名字產生器
    </h1>
    <p>
      選取您需要的條件，並點擊下方按鈕獲取結果。
    </p>
    <div class="options-container">
      <!-- 元件化 -->
      <OptionContainer
        v-for="item in optionsDatas" :key="item.title"
        :optionData="item" :options="options"
      />
    </div>
  </div>
</template>
```

> 這邊傳入 `optionData` 以及 `options` 這兩個 `props`
> `optionData` 用於顯示 `p` 段落以及 `button` 按鈕
> `options` 則用於顯示當前選中的對象

而在 `OptionContainer.vue` 中，則需要接收傳入的 `props`：

```typescript
// OptionContainer.vue 的 script 標籤中

import { Gender, Popularity, Length } from "@/data"; // 引入 data.ts 中的 enum

interface OptionProps {
  // 聲明 props 的類型
  optionData: {
    // 傳入的 props：optionData
    title: string; // title 為字串類型
    cate: string; // cate 為字串類型
    buttons: Gender[] | Popularity[] | Length[]; // buttons 為三種 enum 的陣列
  };
  options: {
    gender: Gender; // gender 為對應的 enum
    popularity: Popularity; // popularity 為對應的 enum
    length: Length; // length 為對應的 enum
  };
}
const props = defineProps<OptionProps>(); // 這邊用 defineProps 接收 props，並設置好類型為 OptionProps
```

接著再將樣式、資料等串接到元件中：

```htmlembedded
<!-- OptionContainer.vue 中 -->

<template>
  <div class="option-container">
    <p>{{ optionData.title }}</p>
    <div class="option-buttons">
      <button
        v-for="option in optionData.buttons" :key="option"
        :class="{ 'active': options[optionData.cate as 'gender' | 'popularity' | 'length'] === option }"
      >
        {{ option }}
      </button>
    </div>
  </div>
</template>
```

> 這邊需注意型別問題
> 在 `:class` 處，需要設置：
> `optionData.cate as 'gender' | 'popularity' | 'length'`
> 否則會噴型別上的錯誤```

最後即可綁定點擊事件，當按下按鈕時將 `options` 的值進行更新
第一步要先新增函數 `clickOption` 當作點擊事件要執行的內容：

```typescript
// OptionContainer.vue 的 script 標籤中

const clickOption = (option: Gender | Popularity | Length) => {
  // 新增函數 clickOption
  if (props.optionData.cate === "gender") {
    props.options[props.optionData.cate] = option as Gender;
  } else if (props.optionData.cate === "popularity") {
    props.options[props.optionData.cate] = option as Popularity;
  } else if (props.optionData.cate === "length") {
    props.options[props.optionData.cate] = option as Length;
  }
};
```

> `option: Gender | Popularity | Length` 設置傳入參數 `option` 的類別
> 接著藉由判斷 `optionData.cate` 的值，分別設定好 `option` 是哪個 `enum`

接著在每個按鈕上綁定點擊事件，並將 `option` 傳入 `clickOption` 函數中：

```htmlembedded
<!-- OptionContainer.vue 中 -->

<template>
  <div class="option-container">
    <p>{{ optionData.title }}</p>
    <div class="option-buttons">
      <button
        v-for="option in optionData.buttons" :key="option"
        :class="{ 'active': options[optionData.cate as 'gender' | 'popularity' | 'length'] === option }"
        @click="clickOption(option)"
      >
        {{ option }}
      </button>
    </div>
  </div>
</template>
```

## 處理搜尋結果

在 `app.vue` 中有一個用來執行搜尋的按鈕
首先需要定義好所有資料內容，可於 `data.ts` 中進行撰寫：

```typescript
// data.ts 中

// 這邊是之前聲明的性別、流行程度以及長度的 enum
export enum Gender {
  GIRL = 'Girl',
  BOY = 'Boy',
  UNISEX = 'Unisex'
}
export enum Popularity {
  UNIQUE = 'Unique',
  TRENDY = 'Trendy'
}
export enum Length {
  ALL = 'All',
  LONG = 'Long',
  SHORT = 'Short'
}

// 這邊往下是要用來查找搜尋結果用的資料內容
export interface Name { // 首先聲明 Name 物件的類型
  id: number; // 有 id 為數字
  name: string; // 有姓名為字串
  gender: Gender; // 有性別為 enum Gender
  popularity: Popularity; // 有流行程度為 enum Popularity
  length: Length; // 有長度為 enum Length
}

export const names: Name[] = [ // 接著聲明所有 names 資料為 Name 類型的陣列
  { // 開始建立一筆一筆的 Name 資料內容
    id: 1,
    name: "Laith",
    gender: Gender.BOY,
    popularity: Popularity.UNIQUE,
    length: Length.SHORT,
  },
  {
    id: 2,
    name: "Jake",
    gender: Gender.BOY,
    popularity: Popularity.TRENDY,
    length: Length.SHORT,
  },
  {
    id: 3,
    name: "Lamelo",
    gender: Gender.BOY,
    popularity: Popularity.UNIQUE,
    length: Length.SHORT,
  },
  ...
];
```

接著回到 `app.vue` 檔案中，撰寫搜尋姓名用的事件與資料：

```typescript
// app.vue 的 script 標籤中

const selectedNames = ref<string[]>([]); // 首先新增一個類型為字串的變數 selectedNames 用於存放搜尋結果

const computedSelectedNames = () => {
  // 新增一個函數 computedSelectedNames 用於篩選結果內容
  const filterNames = names.filter(
    (name) =>
      name.gender === options.gender && name.popularity === options.popularity
  );
  if (options.length !== Length.ALL) {
    // 假設長度的選項不為全部 則使用 filter 根據選項進行取值
    selectedNames.value = filterNames
      .filter((name) => name.length === options.length)
      .map((item) => item.name);
  } else {
    // 假設長度的選項為全部 則直接顯示篩選過姓名與流行程度後的結果
    selectedNames.value = filterNames.map((item) => item.name);
  }
};
```

> 這邊要注意，輸出的結果 `selectedNames` 類型為字串
> 所以 `filter` 後得到的 `Name` 對象，要再通過 `map` 取出其 `name` 的值，設為最終的 `selectedNames` 結果

接著將函數 `computedSelectedNames` 綁定給搜尋結果的按鈕
最後再把 `li` 改用 `v-for` 的方式遍歷 `selectedNames` 呈現出結果：

```htmlembedded
<!-- app.vue 中 -->

<template>
  <h1>
    寶寶英文名字產生器
  </h1>
  <p>
    選取您需要的條件，並點擊下方按鈕獲取結果。
  </p>
  <div class="options-container">
    <OptionContainer
      v-for="item in optionsDatas" :key="item.title"
      :optionData="item" :options="options"
    />
    <button @click="computedSelectedNames()" class="submit-btn">搜索姓名</button>
  </div>
  <ul v-if="selectedNames.length">
    <li v-for="name in selectedNames" :key="name">{{ name }}</li>
  </ul>
</template>
```

## 搜尋結果元件化 CardName.vue

在搜尋結果中的 `li` 因為是完全相同的結構，就很適合製作成元件

在將 `li` 進行元件化前，先為每個 `li` 添加一個 `span` 標籤，綁定刪除姓名的事件：

```htmlembedded
<!-- app.vue 中 -->

<template>
  ...
  <ul v-if="selectedNames.length">
    <li v-for="name in selectedNames" :key="name">
      {{ name }}<span @click="removeName(name)">❌</span>
    </li>
  </ul>
</template>
```

接著撰寫函數 `removeName` 執行的內容：

```typescript
// app.vue 的 script 標籤中

const removeName = (name: string) => {
  const i = selectedNames.value.findIndex((item) => item === name); // 獲取當前目標的索引值
  selectedNames.value.splice(i, 1); // 通過 splice 方式刪除一個 element
};
```

最後就可以進行元件化了
在資料夾 `components` 中新增檔案 `CardName.vue`
並將 `li` 標籤整個拷貝到檔案 `CardName.vue` 中：

```htmlembedded
<!-- CardName.vue 中 -->

<template>
  <li v-for="name in selectedNames" :key="name">
    {{ name }}<span @click="removeName(name)">❌</span>
  </li>
</template>
```

接著通過 interface 聲明好傳入的 props 類型：

```typescript
interface NameProps {
  name: string;
}

const props = defineProps<NameProps>();
```

最後將函數 removeName 通過 emit 的方式傳遞給 CardName.vue：

```htmlembedded
<!-- app.vue 中 -->

<template>
  ...
  <ul>
    <CardName
      v-for="item in selectedNames" :key="item"
      :name="item"
      @removeName="() => removeName(item)"
    />
  </ul>
</template>
```

```htmlembedded
<!-- CardName.vue 中 -->

<template>
  <li>{{ name }}<span @click="emit('removeName')">❌</span></li>
</template>
<script setup lang="ts">
...
const emit = defineEmits<{
  (e: 'removeName'): void // e: 函數名稱 'removeName' : 函數類型為 void
}>()
</script>
```

> emit 宣告類型的方式參考：
>
> ```typescript
> const emit = defineEmits<{
>   // 用物件包裹內容
>   (e: "change", id: number): void; // e: 函數名稱 'change', 參數 id 類型數字 : 函數類型為 void
>   (e: "update", value: string): void; // e: 函數名稱 'update', 參數 value 類型字串 : 函數類型為 void
> }>();
> ```
