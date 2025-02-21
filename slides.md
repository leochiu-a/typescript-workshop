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
- keyword - as、!
- type narrowing
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

<div v-click="3">

- [exercise](https://www.typescriptlang.org/play/?#code/PTAEAcCcHsCMBsCmBbUBGUBaUhpqMHfyoALgIawDOogIW6DR6oKRKAUPQMbQB2ZhRpZAXKAN6h4pRPD4dIAS1YBzUAF9QAXlABteqE0ChIsaADkgfStAFp76ANKABuxeAFdEffcUmRww6foVmNWwcNiijoDWDoBTceZWNvaOhJDOrPry3lra-oEGgPcZgOl64dZ2DgYAFtCEop7y9AC6jCAQMAgooABMWLiAMdqACEagAKpkiJAAkqwAZtBUdIAw-4ws7Jy2vQPDoyr88gDcjHN9gyMAdKzEyIjKBgAyiND69JsLu8QyRypoAAxPV-Pb0DvIxJBSiAAmxyGNl69CAA) 

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

const bird: FlyingAnimal = {
  name: "Eagle",
  canFly: true
};
```

</div>

<div v-click="3">

- [exercise](https://www.typescriptlang.org/play/?#code/PTAEAcCcHsCMBsCmBbUBmUBaUhpqMBSuoAlgHYAuiAZgIYDGiogaEaikCe49g+K6AIRqAKLLh40FonqAQt0DR6oFIlQDD-gSHNAloqA6L0CHKoFPdUADEArvHgAVQskT9Bw0Y1AAFKpFKHjpoSMQAoV63Z8Bz0e5CggBaKgO4B6q400MQAzqSgKGYuAIwAXN4JFgC8oADerqCgxFTGqQBEAILwhHQlADR5oFFU8LYsqQCsAAxdHXX5sIjElISkUakA2iUAEohNpAAWoACS0dqQVMTVNaAlNoQAJqAAarRUpISRJQC6rgC+ANz+YGqAAdEc4ZExcT7miABMqU4fqAsrl8oVitsAEJwWr1ObQVbwFgAJVOiFSvx6cIRkCiAHVoJAANaIPYYtq3B6uAJQOBIVAAFiwoEAWdqASv9AABygGgFUCQ7SkUiRUDSdyeeh8gWRKwwcBRYE5erNfrwVIxSAkADmd1A9QiQkgqtI6uIWvy9SihAAXujQAByKLIJrwW2gAA+duMe0I2mQLvdtuakA1iFtD3uorY4v5guIvAAbgMRvLQaBIgBhSo0ImpAAUAEpgQA+UBx6D7bWU9wRaKxZAsCUx1INoUgxVUZWlDNVImgACyiFq+TN+T1hNKCG0A62Q5n5qtNpKnu9yEHM6H9XTmezoHzRZya-XM+rUWgSAAdEINTmSs3iKAaFvSQBCEp5h75G6UoA)

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

- runtime declaration

```ts
<script setup lang="ts">
const props = defineProps({
  foo: { type: String, required: true },
  bar: Number
})

props.foo // string
props.bar // number | undefined
</script>
```

- type-based declaration

```
const props = defineProps<{
  foo: string
  bar?: number
}>()
```

---

# Vue - defineEmits

<v-clicks>

- runtime 

```ts
const emit = defineEmits(['change', 'update'])
```

- type-based

```ts
const emit = defineEmits<{
  (e: 'change', id: number): void
  (e: 'update', value: string): void
}>()
```

- 3.3+: alternative, more succinct syntax

```ts
const emit = defineEmits<{
  change: [id: number]
  update: [value: string]
}>()
```

- [exercise](https://play.vuejs.org/#eNp9kctOwzAQRX9lZCGVSqVZwKqECqi6gAVU0KU3eUyKW8e2bCcURfl3xg59LFB3nnvvjM54OvZkzLRtkM1Y6gorjAeHvjFzrkRttPXQgcUKeqisrmFE0RFXXJVYCYUrq41Lu35+Pb4/qctaeJeSxlWaDENpHBUeayMzj1QBpKVoob2ptH3gTJAFQoHPcscZzHb486dO20w2yFnsAeg6iKrMcpTQ92Fu0NO88V4reCykKHbUe4VEcT3SahGEdZaPJrFzTKPSZEgPHAmB0CtNzvDYhHlXaFWJzXTrtKLv6UKYs0LXRki078YLrQh2BtEJXial/n6NmrcNTg568YXF7h996/ZB42xl0aFtac2j5zO7QT/Yy8833NP7aNa6bCSlL5gf6LRsAuMQe25USdhnuUj7Eo8s1GbtlnuPyh2WCqAh2cc8Z3T4xYXVT7i307vYx1XP+l/BM8of)

</v-clicks>

---

# Utility Types

- `Pick<Type, Keys>`

```ts {2-3|5|all}
interface Todo {
  title: string;
  description: string;
  completed: boolean;
}
 
type TodoPreview = Pick<Todo, "title" | "completed">;
 
const todo: TodoPreview = {
  title: "Clean room",
  completed: false,
};
```

---

# Utility Types

- `Record<Keys, Type>`

```ts
type CatName = "miffy" | "boris" | "mordred";
 
interface CatInfo {
  age: number;
  breed: string;
}
 
const cats: Record<CatName, CatInfo> = {
  miffy: { age: 10, breed: "Persian" },
  boris: { age: 5, breed: "Maine Coon" },
  mordred: { age: 16, breed: "British Shorthair" },
};
```

- [exercise](https://play.vuejs.org/#eNqtVD9v00AU/ypPXkylEkvA5DqBQju0Q4kKbF5M/Jxca99ZvnNIFSKxIBWlA0JMDDBVUCF1YAEJxKdpaDa+Au/uXMektBNe/O79/d17v3djZz3PW8MSHd8JFGZ5GinshBwgeCCyXAsAfimxaIeO/oVOpcuxyJiUTHBJpsbJeniUJPAaGekoewXLFUhUZQ5pxPsUqMifrCzLRaFgDOogR3hChbqFSFiKq1azK1LsLmrABJJCZOC2PG126wQadG3SB303Moe8R2EK9BX8Zn5ow1jj5VGGPrjbYsBhQ6C7qpWYRSwl7R5pW7HAeziK6ELY6olMe0zWFpkbHfAvwa2KRHHGuA+qKNHkt2iSKJX23C9RqtqusweebVrHWaVWUaWE9Vt7UnCal0kZOoQlp4sUD3Nl2+/bYtoWpal4tm10dVETM8De/j/0e3KkdaHTLZCwDTF0apuKij4qa958tIMjkmtjJuIyJe9rjLsoRVpqjNbtfsljgt3wM2i3zCAZ7z+WmyOFvOKUBao9J8Y/dGiwesBXXX0B93brjomjjlIXL0hxmfAxGxqBxMGtjuYIbPFEBB6dKn3e2TE8GY/N7FqaNTCZBF6+8Ni0pLlwMRyqfRb5NUWgwZFmmTKtJJJTBsObidD7d2MQyUXEKhSUYgUYb3IvdMDfxwPy1lazWzYRaERaR1gMur+SwV1w13X7MXaBGL+BnGlxQl2rgHgpu8DnVQADz/bsikVvrLhd+v+x6CGPMWEcKSyXgRn3paWmxYFrF5Ko0Lmx0livkOsFowotJYkZjCsskqhnEVpSsdgHXmZPsTDp7YMhVUFUNYrqsag19nGor2nm3QbXvAEuPAdXwzaCWXt3jR5ND2an785/nsw/vpm/+qLjSdVe/ox2/nk6Oz36dfj67Pvx2bfp7OXJ7MV09n46Ozz+/eOIrOdvP0GX9fZh/vUD7GJPFDFclS/kODJzWZ7Ksmmpj87kD72PK7Y=)

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

- 利用 if-else、typeof 等等的判斷 <span v-mark.circle.orange="0">縮小 type inference</span>：

```ts
let value: string | null = "Hello, world!";

if (value !== null) {
  // 在這裡，編譯器知道 value 是 string 而不是 null
  console.log(value.length);  // 可以安全地使用 string 的方法
}
```

- 使用 `typeof` 實作 type narrowing

```ts
function printLength(value: string | number) {
  if (typeof value === "string") {
    // value 的型別被縮小為 string
    console.log(value.length);  // 可以安全地使用 string 的方法
  } else {
    // value 的型別被縮小為 number
    console.log(value.toFixed(2));  // 可以安全地使用 number 的方法
  }
}
```

---

# type narrowing 搭配物件使用的進階技巧


- 這個技巧不管是在 function 還是在 vue 中都很實用
```ts
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

# 延伸閱讀

如果想要完整的學習 TypeScript 推薦：

1. [roadmap.sh - TypeScript](https://roadmap.sh/typescript)
2. [TypeScript official handbook](https://www.typescriptlang.org/docs/handbook)
3. [TypeHero - 刷題](https://typehero.dev/tracks/typescript-foundations)
4. [IT 鐵人 - 讓 TypeScript 成為你全端開發的 ACE！](https://ithelp.ithome.com.tw/users/20120614/ironman/2685)


---
layout: center
---

# Q&A


---
layout: center
---

# Thanks you