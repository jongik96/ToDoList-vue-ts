## 목차

### [branch 설명](#1-브랜치)

### [Options API 기반의 Vue2, Vue3 비교해보기](#2-vue2=>vue3)

### [Vu3 - Options API 와 Composition API 비교해보기](#3)


#1 브랜치
## Branch Info

#### vue2-complete : Vue2 + TypeScript  
#### vue3-optionsAPI-complete : Vue3 (Options API) + TypeScript  
#### vue3-todo-compositionsAPI : Vue3 (Composition API) + TypeScript  

TodoApp 을 위와 같은 기술을 적용해 제작했습니다.
위 3가지 브랜치의 프로젝트는 모두 동일하게 동작합니다.  

Vue 에 TypeScript 를 적용해보는 것, Vue2 와 Vue3 의 차이, 그리고 Vue3 의 Options API 와 Compositions API 의 차이를 이해하며 진행했습니다.


#2 Vue2=>Vue3
# TypeScript 를 적용해 Vue2, Vue3 만든 TodoApp 비교

Inflearn 강좌 "Vue.js+TypeScript 완벽가이드" (장기효) 를 참고하며 제작한 **Vue2** 버전의 TodoApp 을 **Vue3 Options API** 기반으로 다시 제작해보았습니다.


## 실행화면

![](https://images.velog.io/images/pji3504/post/7ed4b2e5-fcca-4382-a554-10b7994f5db4/ezgif.com-gif-maker%20(3).gif)

---

## 코드 비교

### TypeScript 사용

두 개의 프로젝트 모두 vue-cli 로 프로젝트를 생성하는 시점에 TypeScript를 적용해 생성했습니다.

Vue 2 버전에서는  
`<script lang="ts">`  
와 같이 script 태그를 설정한 뒤 바로 TypeScript 를 적용해볼 수 있었습니다.

하지만 Vue3 버전의 경우 TypeScript 에서 Vue Component 내의 타입을 올바르게 추론하려면

```
<script lang="ts">
  import { defineComponent } from "vue";
  export default defineComponent({
      // 생략
  })
</script>
```

의 형태로 script 코드를 작성해야합니다.

[defineComponent](https://v3.vuejs-korea.org/ko-kr/api/global-api.html) 는 전달된 객체를 반환하는 것 이외에는 아무것도 하지 않지만, 입력에 관해서는, 반환된 값에는 수동 렌더링 함수, TSX 및 IDE 도구의 지원을 위한 생성자의 합성 유형이 있습니다.

---

### v-model vs Input 이벤트

#### Vue 2

-   App.vue (상위)

```
<!-- template -->
<TodoInput
        :item="todoText"
        @input="updateTodoText"
        @add="addTodoItem"
></TodoInput>
<!-- script -->
<script lang="ts">
  // emit으로 받은 입력값을 data의 todoText에 저장
  updateTodoText(value: string) {
    this.todoText = value;
  },

  // emit 으로 받은 이벤트가 동작하는 부분
  addTodoItem() {
      // 생략 
  },              
</script>
```

-   TodoInput.vue (하위)

```
<!-- template -->
<input
      type="text"
      :value="item"
      @input="handleInput"
      @keyup.13="addTodo"
/>
    <button class="btn" @click="addTodo" type="button">추가</button>

<!-- script -->
<script lang="ts">
  // props 에 대한 type 유효성 검사
  props: {
    item: {
      type: String,
      required: true,
    },
  },
  methods: {
    handleInput(event: InputEvent) {
      const eventTarget = event.target as HTMLInputElement;
      this.$emit("input", eventTarget.value);
    },
    addTodo() {
      this.$emit("add");
    },
  },
</script>
```

-   IME (중국어, 일본어, 한국어 등)가 필요한 언어 (**Vue2**) 의 경우 IME 중 `v-model`이 업데이트 되지 않습니다. 그렇기 때문에 **Input 이벤트**를 사용했습니다.
-   v-model 과 input 이벤트 비교
-   ![](https://images.velog.io/images/pji3504/post/c70a18ce-e88e-4f19-aba0-00fec4c0b8c7/ezgif.com-gif-maker%20(4).gif)
    

#### Vue 3

하지만 Vue3 버전부터 위에서 말한 IME이슈 (한글을 입력할 때 한 글자씩 늦게 binding 됨) 가 해결되었기 때문에 v-model 을 사용해도 무방합니다.

-   App.vue (상위)

```
<!-- template -->
<TodoInput v-model="todoText" @add="addTodoItem"></TodoInput>
<!-- script -->
<script lang="ts">
  data() {
    return {
      todoText: "" as string,
    };
  },
  methods:{
      addTodoItem(){
        // 생략
      }
  }
</script>
```

-   TodoInput.vue (하위)

```
<!-- template -->
<input
      :value="modelValue"
      type="text"
      @input="onInput"
      @keyup.enter="addTodo"
/>
<button class="btn" @click="addTodo" type="button">추가</button>
<!-- script -->
<script lang="ts">
props: ["modelValue"],
methods: {
    // TodoInput 컴포넌트에서 입력하지만 입력한 데이터는 App.vue 에서 관리된다.
    onInput(event: InputEvent) {
      const eventTarget = event.target as HTMLInputElement;
      this.$emit("update:modelValue", eventTarget.value);
    },
}
</script>
```

---

### Vue3 ) Options API 와 Composition API 의 비교

Vue3 의 Composition API 를 학습하면서  
위 TodoApp을 제작한 뒤 다시 Vue3 Composition API 를 기반으로 TodoApp을 제작해봤습니다.

아래 글에서 OptionsAPI 와 Composition API 를 비교하는 글을 볼 수 있습니다.

 [Vue3 ) TodoApp 을 통한 OptionsAPI 와 Composition API 비교

Vue3 ) TodoApp 을 통한 OptionsAPI 와 Composition API 비교 같은 동작을 하는 TodoApp 을 각각 OptionsAPI 와 Composition API 를 이용해 제작했습니다. 전체 코드는 아래 링크를 통해 Github 에서 확인하실 수..

jongik.tistory.com](https://jongik.tistory.com/71)



#3

# Vue3 ) TodoApp 을 통한 OptionsAPI 와 Composition API 비교


#### 같은 동작을 하는 TodoApp 을 각각 OptionsAPI 와 Composition API 를 이용해 제작했습니다.


### 실행화면

![](https://images.velog.io/images/pji3504/post/c65bc166-a5d5-4a1e-adbe-fef95e71d60e/ezgif.com-gif-maker%20(3).gif)

---

## 코드 비교

### setup 사용

#### Options APi

```html
<!-- template -->>
<TodoInput v-model="todoText" @add="addTodoItem"></TodoInput>

<!-- script -->
<script lang="ts">
export default {
  data() {
    return {
      todoText: "" as string,
    };
  },
  methods:{
  	initTodoText() {
      this.todoText = "";
    },
  }
}
</script>
```

#### Composition API

```html
<!-- template -->>
<TodoInput v-model="state.todoText" @add="addTodoItem"></TodoInput>

<!-- script -->
<script lang="ts">
import { defineComponent } from "vue";
export default defineComponent({
  setup() {
    // options API 에서의 data 속성과 동일함
    const state = reactive({
      todoText: "" as string,
    });
  
    // options API 에서의 methods
  	const initTodoText = () => {
      state.todoText = "";
    };
  
  	return { state, initTodoText };
  }
})
</script>
```

- Options API 와 다르게 Composition API 에서는 setup() 함수 내부에 data 속성과 method, lifeCycle hook, watch 를 입력합니다.
그 다음 setup() 함수내에서 생성한 함수들을 return 해주어야 해당 함수들을 사용할 수 있습니다.

- 또한 setup() 함수 내에서는 this 의 사용이 불가능합니다.

- 위 예시에서 state 함수 내에서 data 속성을 정의했기 때문에 data 속성은 ```state.todoText``` 로 data 를 가져올 수 있습니다. 

- defineContent 는 TypeScript 적용을 위해 사용했습니다.


### Props

props 를 사용하는 방법에 있어서도 차이가 있습니다.

#### Options API

- App.vue

```html
<!-- template -->
<TodoListItem
           	v-for="(todoItem, index) in todoItems"
           	:key="index"
            :index="index"
            :todoItem="todoItem"
></TodoListItem>

<!-- script -->

<script lang="ts">
import TodoListItem from "./components/TodoListItem.vue";
// 생략
  data() {
    return {
      todoItems: [] as Todo[],
    };
  },
</script>
```

- TodoListItem.vue

```html
<!-- template -->
<span class="item"@click="toggleItem">{{
      todoItem.title
    }}</span>

<!-- script -->
<script lang="ts">
  props: {
    todoItem: Object as PropType<Todo>,
    index: Number,
  },
  methods:{
  	checkProps(){
  		console.log(this.todoItem);
  	}
  }
</script>
```

#### Composition API

- App.vue

```html
<!-- template -->
<TodoListItem
            v-for="(todoItem, index) in state.todoItems"
            :key="index"
            :index="index"
            :todoItem="todoItem"
></TodoListItem>

<!-- script -->
<script lang="ts">
import TodoListItem from "./components/TodoListItem.vue";
// 생략
  setup() {
  	const state = reactive({
      todoItems: [] as Todo[],
    });
  
  	return { state };
  }
</script>
```

- TodoListItem.vue

```html
<!-- template -->
<span class="item" @click="toggleItem">{{
      todoItem.title
    }}</span>

<!-- script -->
<script lang="ts">
  props: {
    todoItem: Object,
    index: Number,
  },
  setup(props, context) {
    const checkProps = () => {
      console.log(props.todoItem);
    };
  	
  	return { removeItem };
  }
</script>
```

Vue2 버전에서부터 사용하던 Options API 에서는 상위 컴포넌트에서 내려받은 props 에 this 를 사용해 접근했습니다.
하지만 위에서 언급한것처럼 Composition API 의 setup() 내부에서는 this 사용이 불가능합니다.

따라서 setup() 의 props 인자를 이용해 this 가 아닌 ```props``` 키워드를 이용해 props 에 접근할 수 있습니다.

이는 this 의 무분별한 사용을 막게 해줍니다.


### Emit 이벤트


#### Options API

- App.vue

```html
<!-- template -->
<TodoListItem
            v-for="(todoItem, index) in todoItems"
            :key="index"
            @remove="removeTodoItem"
            @toggle="toggleTodoItem"
></TodoListItem>

<!-- script -->
<script lang="ts">
import TodoListItem from "./components/TodoListItem.vue";
// 생략
  methods:{
  	removeTodoItem(index: number) {
      this.todoItems.splice(index, 1);
    },
  }
</script>
```

- TodoListItem.vue

```html
<!-- template -->
 <button type="button" @click="removeItem">삭제</button>

<!-- script -->
<script lang="ts">
  methods: {
    removeItem() {
      this.$emit("remove", this.index);
    },
  }
</script>
```

#### Composition API

- App.vue

```html
<!-- template -->
<TodoListItem
            v-for="(todoItem, index) in state.todoItems"
            :key="index"
            @remove="removeTodoItem"
            @toggle="toggleTodoItem"
></TodoListItem>

<!-- script -->
<script lang="ts">
import TodoListItem from "./components/TodoListItem.vue";
// 생략
  setup(){
  	const removeTodoItem = (index: number) => {
      state.todoItems.splice(index, 1);
    };
  
  	return { removeTodoItem };
  }
  
</script>
```

- TodoListItem.vue

```html
<!-- template -->
<button type="button" @click="removeItem">삭제</button>

<!-- script -->
<script lang="ts">
  setup(props, context) {
  	const removeItem = () => {
      context.emit("remove", props.index);
    };
  
  	return { removeItem };
  }
</script>
```

Options API 에서는  ```this.$emit``` 을 이용해 상위 컴포넌트로 이벤트를 전달 할 수 있었습니다.

Composition API 에서는 setup() 내부에서 ```this``` 사용이 불가능합니다.

따라서 setup() 함수의 두 번째 인자인 context 를 이용해
```context.emit()``` 와 같이 emit 을 사용할 수 있습니다.



### computed 사용법

할 일의 상태 변경을 위한 클래스 바인딩을 예시로 들겠습니다.

#### Options API

```html
<!-- template -->
<span :class="todoItemClass" @click="toggleItem">{{
      todoItem.title
  }}</span>

<!-- script -->
<script lang="ts">
  computed: {
    // style 조건
    todoItemClass(): string | null {
      return this.todoItem.done ? "complete" : null;
    },
  },
</script>
```

#### Composition API

```html
<!-- template -->
<span :class="todoItemClass" @click="toggleItem">{{
      todoItem.title
  }}</span>

<!-- script -->
<script lang="ts">
import { computed } from "vue";
  setup(){
    const todoItemClass = computed(() => {
        return props.todoItem.done ? "complete" : null;
    });
  	
  	return { todoItemClass };
  }
</script>
```

Options API 에서는 ```computed: {}``` 를 이용해 함수를 바로 정의할 수 있었습니다.

하지만 Composition API 에서는 computed 속성을 import 한 뒤, 
함수표현식을 통해 computed 를 선언합니다.

그리고 마찬가지로 해당 함수를 return 해주어야 사용할 수 있습니다.


### created => X

Options API 와 Composition API 의 라이프사이클 훅은 차이가 있습니다.
다음은 페이지 진입 시 바로 목록을 조회하기 위한 코드입니다.

#### Options API

```javascript
created() {
    this.fetchTodoItems();
},
```

#### Composition API

```javascript
onMounted(() => fetchTodoItems());
```

Options API 의 라이프사이클 훅엥서 사용되는 beforeCreate, created 는 Composition API 의 라이프사이클 훅에서는 필요가 없습니다.

setup 은 beforeCreate 와 created 훅 사이에 실행되는 시점이기 때문에 두 가지 훅은 정의할 필요가 없습니다. 
( setup 직전에 beforeCreate(), setup 직후에 created() )



[참고자료 - Vue3 공식문서](https://v3.ko.vuejs.org/guide/composition-api-lifecycle-hooks.html)


----

Options API 와 Composition API 두 가지 방법 모두 저의 코딩스타일로 진행했기 때문에 보시는 분들마다 차이가 있을거라 생각합니다.

비효율적인 코드가 있었다거나, 바람직하지 못한 방법으로 작성한 코드를 발견하셨다면 언제든지 댓글로 알려주시면 감사하겠습니다.

간단한 TodoApp 을 두 가지 방법으로 만들어보면서 Options API 와 Composition API 의 차이를 조금 더 이해할 수 있었던 것 같습니다.


