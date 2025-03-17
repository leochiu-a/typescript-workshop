---
theme: seriph
background: https://cover.sli.dev
title: TypeScript Workshop
class: text-center
transition: slide-left
---

# TypeScript Workshop

Leo Chiu 2025/02

<div @click="$slidev.nav.next" class="mt-12 py-1" hover:bg="white op-10">
  Let's start <carbon:arrow-right />
</div>

<div class="abs-br m-6 text-xl">
  <button @click="$slidev.nav.openInEditor" title="Open in Editor" class="slidev-icon-btn">
    <carbon:edit />
  </button>
  <a href="https://github.com/slidevjs/slidev" target="_blank" class="slidev-icon-btn">
    <carbon:logo-github />
  </a>
</div>

---
transition: fade-out
---

# Outline

- What is TypeScript?
- Basic Type
- Function
- Vue: `definedProps`, `defineEmits`
- Utility Types
- Keyword - as、!
- Type narrowing
- Summary
- (Bonus) Generic
- 延伸閱讀

<style>
h1 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>

<!--
Here is another comment.
-->
---
layout: center
transition: slide-up
---

# What is TypeScript?


---
transition: slide-up
---

# What is TypeScript?

- TypeScript 是由微軟進行開發和維護的一種開源的程式語言
- JavaScript Superset，在 JavaScript 的基礎上擴展特性
- 強型別程式語言
- 瀏覽器不能執行，必須編譯成 JavaScript 才能執行

<br>

<img src="/src/ts-compile-to-js.png" width="400px"/>


---
transition: slide-up
---

# 如何執行 TypeScript

<v-clicks>

- 對於瀏覽器來說轉換成 `.js` 才能執行
- 打包的方法
  - go-based (esbuild): vite、tsup
  - rust-based: rolldown、rspack、rsbuild
- Nuxt 的底層是 vite
- bun、deno 原生支援，近期 node23 支援了 TypeScript，但只支援部分

</v-clicks>

---
transition: slide-up
---

# Basic Type - 1 


<div v-click="1">

- Primitives: `string`、`number`、`boolean`

```ts
const str: string = 'Hello';
const num: number = 123;
const isFade: boolean = true;
```

</div>

<div v-click="2">

<br>

- Array: `Array<T>`、`[]`

```ts
const arr: Array<string> = ['Leo', 'Justin'、'Andy'];
const arr: string[] = ['Leo', 'Justin'、'Andy'];
```

</div>

<div v-click="3">

<br>

- Object: `interface`、`type`、`{}`

```ts {1}
const obj: { first: string } = { first: 'Andy'};
const obj: Name = { first: 'Andy'};
```

</div>

<br>

<div v-click="4">

- 以上稱作 <span v-mark.orange>type annotation</span>

</div>


---
transition: slide-up
---

# Basic Type - 2

<div v-click="1">

- `null` and `undefined`

```ts
let u: undefined = undefined;
let n: null = null;
```
</div>

<br>

<div v-click="2">

- `void`

```
type MyFunc = (): void;
```

</div>

<br>

<div v-click="3">

- `any` (`strict: true` 禁止寫 `any`)

```ts
const obj: any = { a: '123' };
```

</div>

<br>

<div v-click="4">

- [exercise](https://www.typescriptlang.org/play/?#code/PTAEAcCcHsCMBsCmBbUBGUBaUhpqMHfyoALgIawDOogIW6DR6oKRKAUPQMbQB2ZhRpZAXKAN6h4pRPD4dIAS1YBzUAF9QAXlABteqE0ChIsaADkgfStAFp76ANKABuxeAFdEffcUmRww6foVmNWwcNiijoDWDoBTceZWNvaOhJDOrPry3lra-oEGgPcZgOl64dZ2DgYAFtCEop7y9AC6jCAQMAgooABMWLh4gMbWgKdygNByNLS9gDD-jCzsnLZkiJCsxMj5rLbIAZDKBgAyiND6zGwcoMQy+RLScipoAAynQ9uj45AAkqwAZtB8gmMTUzPiMUcKy-w+mjek2miCSWj2oPo5SAA) 

</div>

---
transition: slide-up
---

# interface & type

<div v-click="1">

- interface: 只能用來<span v-mark.orange>定義物件</span>

```ts
interface Person {
  name: string;
  age: number;
}

const person: Person = {
  name: "John",
  age: 30
};
```

</div>

<div v-click="2">

- interface 可以 `extends`

```typescript
interface Employee extends Person {
  jobTitle: string;
}

const employee: Employee = {
  name: "Alice",
  age: 28,
  jobTitle: "Developer"
};
```

</div>

---

# interface & type

<div v-click="1">

- type: 聯合型別（Union Type）

```ts
type StringOrNumber = string | number;

let value: StringOrNumber;
// value 可以是 string 或是 number
value = "Hello";
value = 123;
```
</div>

<div v-click="2">

- type: 交叉型別（Intersection Type）

```ts
type Animal = { name: string };
type Bird = { canFly: boolean };

type FlyingAnimal = Animal & Bird;

// birder 必須同時存在 name 跟 canFly，不然會有 TS error
const bird: FlyingAnimal = {
  name: "Eagle",
  canFly: true
};
```

</div>

<div v-click="3">

- [exercise](https://www.typescriptlang.org/play/?#code/PTAEAcCcHsCMBsCmBbUBmUBaUhpqMBSuoAlgHYAuiAZgIYDGiogaEaikCe49g+K6AIRqAKLLh40FonqAQt0DR6oFIlQDD-gSHNAloqA6L0CHKoFPdUADEArvHgAVQskT9Bw0Y1AAFKpFKHjpoSMQAoV63ZbdBoyYHOou6e9DZ2Dv5mLu4goIAWioDuAequNNDEAM6koChRogCMAFx8Aeb0ALygAN6uoKDEVMZFAEQAgvCEdE0ANDWg6VTwtixFAKwADBNjPbWwiMSUhKTpRQDaTQASiAOkABagAJIZ2pBUxJ1doE02hAAmoABqtFSkhGlNALquAL4A3DFgakAAdEcFJpTLZEouABMRScpVAFWqtXqjUuACE4N1ejtoMd4CwAErPRBFKFTbG4yDpADq0EgAGtEDdSSNvn9XLEoHAkKgACxYUCALO1AJX+gAA5QDQCqA0dpSKQ0qBpME2PRpbK0lYYOB0giqr1BrN4EVMpASABzH6gXqpISQI2kE3Ec21XrpQgALxJoAA5OlkAN4F7QAAfb3GG6EbTIQMhr2DSCmxBev6-JVeVVy4i8ABucyWOqRoDSAGF2jR6UUABQASgRAD5QFnoLcLWz3KkMllkCx02kij3iPm9VQDc0Sx16aAALKIbq1Z21a105oIbQzi5zjcu92epphiPIWcbue9Yul8ugat1qpH48b9vpaBIAB0QlNFaa-dANDPTIAhE0qz+WovjZIA)

</div>

---

# Function

如果函式的參數是可選的，我們可以使用 `?` 來表示。例如，我們想要創建一個函式，它可以接受兩個數字參數，也可以只接受一個：

```ts
type AddOptionalFunction = (a: number, b?: number) => number;

const addOptional: AddOptionalFunction = (a, b = 0) => {
  return a + b;
};

console.log(addOptional(5));    // 輸出 5
console.log(addOptional(5, 3)); // 輸出 8
```

- [exercise](https://www.typescriptlang.org/play/?#code/PTAEiztRK-0X8VHh9Ro9UKRKoDiAnAphgLgSwHYDmAUNgJ4AOGqmOBJxIogZ9qB-zqIbdqHMQMYB7fAGdunLNgBcNCfVABeUAAp8AQwC2GADShVhaooCMABgCUCgHygA3sVChM2AK5p8oAAYAJDABsfAnQASazVNAF8AQlAAQX0g6z0MMPcAbmIwtMYwQBo7QDMoji4eWH4hYQEfDAA6f0IlcRwlACJon1w+DEbTUxT7JkAOeMAvxVBG7z8AmNb2qNjtUBNGkpFyqpq6riaAIQEAI0adEzNu0H6hkd9-HS3t6bi542NGoA)


--- 

# Vue - defineProps

<div v-click="1">

- runtime declaration

`lang="ts"` 在 Vue 使用 TypeScript 使用時必須要加，不然 Vue 不知道你是用 TS 還是 JS

```ts {1-2|null}
<script setup lang="ts">
const props = defineProps({
  foo: { type: String, required: true },
  bar: Number
})

props.foo // string
props.bar // number | undefined
</script>
```

</div>

<div v-click="2">

- type-based declaration <span v-mark.orange="2">(recommended)</span>

```ts
const props = defineProps<{
  foo: string
  bar?: number
}>()
```

</div>

---

# Vue - defineEmits

<div v-click="1">

- runtime 

```ts
const emit = defineEmits(['change', 'update'])
```

</div>

<div v-click="2">

- type-based <span v-mark.orange="2">(recommended)</span>

```ts
const emit = defineEmits<{
  (e: 'change', id: number): void
  (e: 'update', value: string): void
}>()
```

</div>

<div v-click="3">

- 3.3+: alternative, more succinct syntax <span v-mark.orange="3">(recommended)</span>

```ts
const emit = defineEmits<{
  change: [id: number]
  update: [value: string]
}>()
```

</div>

<div v-click="4">

- [exercise](https://play.vuejs.org/#eNqFU82O0zAQfpWRhZRW6o/4OWXTamHVA0jAaukNc0jTSddbx45sp7SqetgriGeAK+KKxIHnYYG3YOykf9Kye0r8zTfjb8bfrNnTsuwtKmQxS2xmROnAoqtKkKmaDThzlrMhV6IotXGwBoM5bCA3uoCI0qJd6EwXZYP3+v7gq0YnXHGVaWUdZJUxqNw4ncDAl0msM0LNhq32nuPSiaXoW66A7pLpBGUM0c3Xz7++fI86sEhlhQSkwpQyFSqCTeeY+vv625+fHw6oztzKu/n04+/1xwPey4sxsbh6txdzmaqpxDMpsnktukXy2jAYwtoX2/fTC0WIQHGuqEjSr0dJg6ODw4LUOqQTQBLm5P8AYt+unzF9OKuxU626mb+ySyjFjkXUrD5VSvoHZVmH3olE52LWu7Ja0WMGiZxldJuQaF6XTlBTnMW1eB9LpdTvXwTMmQrDhELOJWbzW/Aru/QYZ+cGLZoFcraLudTM0NXh0ZtXuKT/XbDQ00oS+47gBVotK6+xpj2rfNvmgBfUPg9eI9eM7WjpUNltU0dCvbGyIMbDHq2fnzOypJ///yay7+Jx70nIo+ek4W7tfM+STDEXCs+NLm2y3pCxg/trdFQI16B3+2MqFmDdSiLVnQpLkVUMucTlCczSMoaHj0p6hsBt2Iturg2xBRUCocIScQbxHFcNWht0l0V5k8o5reA0WI1YD5D0tSKttk6j1fCZ7YMk2p91AHthi2BD02nK9et6W1V9klV30/wdu3XzD4aYfi8=)

</div>


---

# Utility Types

- `Pick<Type, Keys>`

Pick 是 TypeScript 的一個內建工具類型，用來從一個型別中選擇某些屬性，並創建一個新型別，只包含這些屬性。

```ts {2-3|5|all}
interface Todo {
  title: string;
  description: string;
  completed: boolean;
}
 
// 我們希望 TodoPreview 只包含了 Todo 的 title 跟 completed 屬性
type TodoPreview = Pick<Todo, "title" | "completed">;
 
const todo: TodoPreview = {
  title: "Clean room",
  completed: false,
};
```

---

# Utility Types

- `Record<Keys, Type>`

`Record` 可以幫助你根據某個 `Keys` set 來生成一個具有這些 `Keys` 且值為指定型別 `Type` 的物件。

```ts
type CatName = "miffy" | "boris" | "mordred";
 
interface CatInfo {
  age: number;
  breed: string;
}
 
// ｃats 這個物件的 Keys 為 CatName，並且每個隻貓都有同樣的 CatInfo
const cats: Record<CatName, CatInfo> = {
  miffy: { age: 10, breed: "Persian" },
  boris: { age: 5, breed: "Maine Coon" },
  mordred: { age: 16, breed: "British Shorthair" },
};
```

- [exercise](https://play.vuejs.org/#eNqtVc1OFEEQfpXKXEYT3EnU07isvxz0oAT1Npdxp3ZpmOmezPTgmnUTEzVBlwRivEiInhAQxJ+DEvHnZXYAT76C1d3D7ADCyb1sdVV11VdVX/V0rctxXJvJ0HKtusQoDn2JDY8D1K+KKFYCgJulmIx6lvrzrEIXYxKxNGWCp2SqnIyHQ0HqTiUiHdNmwmIJKcoshtDnbbooyZ+sLIpFIqEL8kGMcJcSjSeixUIcMZoJEeL4MAf0oJWICOyao8x2GUCBLk3qoGojs8ebdE2CKsGtxodR6Cq83I/QBfuGmORwTaA9opQY+Swk7RRpa4HAS9jxqSCsNUWkPHoXhpErHXCPwC2S+EHEuAsyyVDHN2hafpiaczvDVJZ2Fb3umKY1rBFqFWVqsXZtKhWc5qVDehZhiamQ5FYsTftdk0zZ/DAU929oXZlU35nE5vQ/9FNpR+k8azxBwjaDnlXapJ+0URrz2O2b2CG5NEYiyELyPsE4gakIM4XRuF3JeECwK34a7XU9SMbbd9KxjkRecMoAVZ497e9ZNFg14ONKH8I9Vzuv71FHqYv7pDhK+IDNaIHEybMNxRG4zlui7tCp0MeNm5on3a6eXU2xBnq9uhMPPcYMafZdNIdKn2F8RRGocKSaJgsLieSQwcyZllD7d2rST4c3RiChEKeB8Sr3PAvcaXxA3sqqd8sEAoVI6QiLRncgGFwE+7JqPwY2EOOvIWdK7FHXCiBOyPbxOQXAumN6dsyiV1bcLP3/WHSPB9hiHOlanNb1uI8sNS0OnLiQRIXGqdOV9fK4WjDKUJMpMYNxiUnLbxqEhlQscIFn0T1MdHjzYKQyIapqRfFYlBrzOJRl6nmPgq3fABsegq1ga0GvvX2BHk0H8s3F3Z9reysv9p59VvdJNXr4p7V76/18c25ndmGwvTzY6udP1/JH/fx1P59d/vN9jqy7L1dhnDWnYe/rG5jApkgCOC6e1ldfxXxpNX/6eLD9ZbD1Pp9/t7P0bPDjF0XMZz/qynVQQ+0i86eNnUcrHseOnu/h6RYpDk83n/9A8PPZNQW27FK+/W13vW96sLv4ZGfj+e9XC1TUYOst4YLiiwUEzYA6mPVQDqv3FxHhhqY=)

---

# keyword - `as`

- `as` 強制轉換

這樣可以確保 element 被轉換為 HTMLButtonElement 類型，並且你可以安全地訪問 HTMLButtonElement 上的屬性。

```ts
let element = document.getElementById("myButton");
let button = element as HTMLButtonElement;
button.disabled = true;
```

但強制轉換不是首選的做法，首選還是 type annotation 跟 type inference

```ts
let element: HTMLButtonElement = document.getElementById("myButton");
let button = element;
button.disabled = true;
```

---

# keyword - `!`

假設你有一個方法，它的參數可以是 null 或 undefined，但你知道你只會傳遞一個有效的值，這時你也可以使用 ! 來防止 TypeScript 進行空值檢查：

- `!` Non-null assertion

```typescript
function greet(name: string | null) {
  console.log("Hello, " + name!.toUpperCase());
}

greet("John"); 
```

常見的範例還有像是 `arr.find()`

```ts
const people: Person[] = [
  { name: "Alice", age: 30 },
  { name: "Bob", age: 25 },
  { name: "Charlie", age: 35 }
];

let person = people.find(p => p.name === "Bob")!;
console.log(person.age);  // 輸出: 25
```

---

# type narrowing 型別縮小

<div v-click="1">

- 利用 if-else、typeof 等等的判斷式 <span v-mark.circle.orange="1">縮小 type inference</span>：

```ts
let value: string | null = "Hello, world!";

if (value !== null) {
  console.log(value.length);  // 可以安全地使用 string 的方法
}
```

</div>

<br>


<div v-click="2">

- 使用 `typeof` 實作 type narrowing

```ts
function printLength(value: string | number) {
  if (typeof value === "string") {
    console.log(value.length);  // 可以安全地使用 string 的方法
  } else {
    console.log(value.toFixed(2));  // 可以安全地使用 number 的方法
  }
}
```

</div>

---

# type narrowing 搭配物件使用的進階技巧


- 這個技巧不管是在 function 還是在 vue 中都很實用

```ts {1,3-4}
interface TextMessage {
  type: 'text';
  message: string;
}

interface ProductMessage {
  type: 'product';
  product: Product;
}

type Message = TextMessage | ProductMessage;

const message: Message = /** msg */;

if (message.type === 'text') {
  console.log(message.message)
} else {
  console.log(message.product)
}
```

- [exercise](https://play.vuejs.org/#eNqNVM9rE0EU/leec1mFZnPQU9wUVAoqqEVznMuymcStu7PLzGwaiQFBT/XgQb2IUhSLkYI9VpTiP5N0k//CN7M/09S0EMj8+Oa9733vezsit+LYHiSMtIgjPeHHCiRTSQyBy/ttSpSkZJNyP4wjoeABk9LtM+iJKATLbuZ7HcC6WaJGoJ7HrAB39HpcPtFXGku5F3GpQLGhypGtpSdtGFEOJlQLLA2zNvRBWICtuywIIovycRUtFlE38S4TMEdmMfNNK0MgxleBBp3+2E8PJpgCAJPgz2lmIqEkuFEsjANXMdwBOFcaDZgfvpme/E3fTzIJuCtEtOvzPqQfX5/uvZwdf5//fAeGOMyP9yFLsHh1Mnt7NP1zsPhylH77Pfs0ST8cLj5/nf7ag0Yji971B2aBy6ILrVwK3aZKRUqguQa5rFAJdppZAqdZK4psYP9R2J7ft3dkxNEkRiBKvCiM/YCJR7HyUXhKSukocbG43fvmTImEGYHNm6fMe3bO+Y4c6jNKtgWTTAyQVHmnXNFnKrveevIQq6xdhlhJgOg1l4+ZjIJEc8xgtxPeRdo1nGF7zxgX+9SRW0PFuCyK0kRN9w2eEjT6nTWlV3Sv2zcK16CKtTm53KBdPEKgHdhlPZ+zbRHF0jEkyuGovUQKm1ev4citM29lr9GoiGLn/zDGMiqL/B+fe8s243Pm1YqxkJmtJMrhc8VEz/UYdCoXrw4/FlCrTyqB7cIzM/xVCNSiZu9zJ94EKic+f1BEYkOj/4r67SVyL87kMd+zFRJ59uxbUidMxv8Azmb4Mw==)

---

# Summary

<v-clicks>

- TypeScript 解決了 JavaScript 弱型別容易造成系統不穩定的痛點
- 在寫 TypeScript 時以 type inference 為優先，不用一定要使用 type annotation
- TypeScript 現今主要是用來做 type-checking 的工具
  - vue 的官方有一個工具叫做 `vue-tsc`，通常可以搭配 git hooks 或 CI 做 type checking
- 這次沒有提到 generic，generic 在設計泛用元件時非常好用
- `enum` 有很多缺點，之後不建議寫 `enum`

</v-clicks>

---

# (Bonus) Generic

- 利用泛型建立一個 Tabs 元件，可以讓 `value` 允許是 `string | number`，不一定需要在 Tabs 上宣告型別，而是使用 type inference 的方式搭配 generic 推斷出型別是否合法：

```ts
<script setup lang="ts" generic="T extends string | number">
defineProps<{
  tabs: Array<{ text: string; value: T }>
  activeKey: T
}>()

const emit = defineEmits<{
  (e: 'onChangeTab', activeKey: T): void
}>()
</script>
```

```ts
// OK
<Tabs :tabs="[{ text: '飛機'; value: 'airplane' }]" />
<Tabs :tabs="[{ text: '飛機'; value: 123 }]" />

// error
<Tabs :tabs="[{ text: '飛機'; value: true }]" />
```

---

# 延伸閱讀

如果想要完整的學習 TypeScript 推薦：

1. [roadmap.sh - TypeScript](https://roadmap.sh/typescript)
2. [TypeScript official handbook](https://www.typescriptlang.org/docs/handbook)
3. [TypeHero - 刷題](https://typehero.dev/tracks/typescript-foundations)
4. [IT 鐵人 - 讓 TypeScript 成為你全端開發的 ACE！](https://ithelp.ithome.com.tw/users/20120614/ironman/2685)
5. [TypeScript 新手指南](https://willh.gitbook.io/typescript-tutorial)


---
layout: center
background: https://cover.sli.dev
---

# Part 2 - 實戰 TypeScript

--- 

# import 的 module 沒有型別時怎麼辦

<div>

也許你會遇到 `xxx` module 沒有型別的情境，有很多套件基礎是 JavaScript，而這些套件無法或是短期內無法轉換成 TypeScript：

```ts {all|1-3}
// Could not find a declaration file for module 'lodash'.
// Try `npm i --save-dev @types/lodash` if it exists or add a new
// declaration (.d.ts) file containing `declare module 'lodash';`
import { get } from 'lodash';
```

- GitHub repo: https://github.com/DefinitelyTyped/DefinitelyTyped

如果 GitHub 上有定義型別，就可以直接執行以下指令：

```bash
pnpm i -D @types/lodash
```

</div>

---

# 如果沒有 `@types/xxx` 怎麼辦？

<v-clicks>

- `declare module 'xxx' {} `
- 如何定義型別 `import { get } from 'lodash'`

```ts {1-2,7|3-6}
declare module 'lodash' {
  export function get(obj: any, path: string): any;
  export function get<TObject extends object, TKey extends keyof TObject>(
    object: TObject | null | undefined,
    path: TKey | [TKey]
  ): TObject[TKey];
}
```

</v-clicks>


---

# 第三種引入型別的方法

<v-clicks>

- 有些套件如果已經是 TypeScript，在 `import` 的時候就會有型別，舉例來說 `@vueuse/core` 原本就有型別
- `package.json` 中的 `types` 即是型別的路徑
- 有些套件的型別不是放在 `index.d.ts` 的時候，需要在 `tsconfig.json` 中註冊：
  ```ts
  // tsconfig
  {
    "compilerOptions": {
      "types": ["<module-name>"]
    }
  }
  ```

</v-clicks>


---

# 如何設定 global 型別

- 例如 `lang()` 或是 `_` 等等全域的 variables、functions
- `declare global`

```ts
declare global {
  const _: typeof import('lodash');
}
```

- 如果是在 Vue `<template>` 中使用的話，需要定義在 `shims-vue.d.ts` 中

```ts
declare module 'vue/types/vue' {
	interface Vue {
		kkday: typeof import('@/share/js/libs/KKdayUtils').kkday;
		_: typeof import('lodash');
	}
}
```


--- 

# Vue Component 如何設定型別

- `Vue.PropType`
- `defineProps`

```ts {1|3-9}
/** @import { Message } from 'Message.type.ts' */

defineProps({
  message: {
    /** @type {Vue.PropType<Message>} */
    type: Object,
    required: true,
  },
}
```

<!-- CityItem, CategorySlider -->

--- 

# Vuex 如何設定型別

- 自定義型別

```ts
declare global {
  type VuexGetters<State, Getters> = {
    [K in keyof Getters]: (state: State, getters: Getters) => Getters[K];
  };

  type VuexActionTrees<State> = import('vuex').ActionTree<State, State>;
}
```

- 定義 `RootState` 

```ts
/** @type {RootState} */
const state = { /** */ }
```

- 定義 getters

```ts
/** @type {Getters} */
const getters = { /** */ }
```

--- 

# 定義 Vuex 的型別

```ts
// 定義初始的 state 型別
interface RootState {
  count: number;
}

// 定義 getter 的 state 型別，需要 extends RootState
interface GettersState extends RootState {
  doubleCount: number
}

// 定義 vuex 的 getters 型別
export type Getters = VuexGetters<RootState, GettersState>;
```


---
layout: center
---

# Q&A


---
layout: center
---

# Thanks you