<script setup>
// import HelloWorld from './components/HelloWorld.vue';
// vue3 컴포넌트 api
import { reactive, computed } from 'vue';

// reactive 함수 : 객체를 반응성으로 만들며 값이 변경되면 리렌더링이 됨
const data = reactive({
  newItem: '',
  items: [],
});

// computed 함수: data.items가 변경될 때만 함수가 실행됨
const totalItems = computed(() => data.items.length);
const isCompleted = computed(
  () => data.items.filter((item) => item.completed).length
);

// 할 일 완료 여부

// 할 일 추가, 함수표현식
const addItem = () => {
  if (data.newItem !== '') {
    data.items.push({
      id: data.items.length + 1,
      text: data.newItem,
      completed: false,
    });
    data.newItem = '';
  }
};

const deleteItem = (id) => {
  const itemToDelete = data.items.find((item) => item.id === id);
  // console.log(itemToDelete);

  let index = data.items.indexOf(itemToDelete);
  console.log(index);

  data.items.splice(index, 1);
};
</script>

<template>
  <main class="app">
    <!-- <HelloWorld /> -->
    <h1>simple to-do list</h1>
    <div class="todo_count">
      완료: {{ isCompleted }} / 할 일: {{ totalItems }}
    </div>
    <div class="todo_add">
      <!-- v-model 디텔기브로 폼 요소와 데이터를 양방향 연결 -->
      <!-- v-on :keyup.enter : 엔터를 쳐서 Add가 되도록 만들기 -->
      <input
        type="text"
        v-on:keyup.enter="addItem()"
        v-model="data.newItem"
        placeholder="할 일을 입력하세요"
        title="할 일을 입력하세요"
      />
      <button type="button" class="add_btn" v-on:click="addItem()">Add</button>
    </div>
    <ul class="todo_list">
      <!-- 리스트마다 고유 id를 key 속성에 단방향 연결 -->
      <li
        v-for="(item, index) in data.items"
        v-bind:key="item.id"
        v-bind:class="{ completed: item.completed }"
      >
        <!-- 변수 데이터와 속성 연결시 v-bind 사용 -->
        <!-- 라벨 클릭 시 for로 연결되어 체크박스가 클릭되며, true, false가 발생하며, v-model로 연결된 속성에 들어감 -->
        <input
          v-bind:id="`check${item.id}`"
          v-model="item.completed"
          type="checkbox"
        />
        <label v-bind:for="`check${item.id}`">{{ item.text }}</label>
        <button
          v-on:click="deleteItem(item.id)"
          class="remove_btn"
          type="button"
        >
          Remove
        </button>
      </li>
    </ul>
  </main>
</template>

<style scoped>
.app {
  padding: 20px;
}
.app h1 {
  font-size: 30px;
  font-weight: 700;
  color: var(--text-color-900);
}
.app .todo_count {
  margin: 10px 0;
}
/* .app .todo_add {
  display: flex;
} */

.app .todo_add input[type='text'] {
  height: 40px;
  border: 1px solid #ddd;
  width: calc(100% - 60px);
  /* flex-grow를 사용하면 기본 크기가 정해져 있기 때문에, 인풋 타입의 사이즈보다 사이즈가 작아지면 줄어들지 못함 */
  /* flex-grow: 1; */
  padding: 0 10px;
  margin-right: 10px;
  border-radius: 4px;
}
.app .todo_add .add_btn {
  height: 40px;
  padding: 0 10px;
  background: #333;
  color: var(--text-color-100);
  border: none;
  border-radius: 4px;
}

.app .todo_list {
  margin-top: 20px;
}
.app .todo_list li {
  margin-bottom: 10px;
  display: flex;
  /* 왼쪽으로 인풋이 있기 때문에 갭을 주면 그쪽에도 공백이 생김 */
  /* gap: 20px; */
}
.app .todo_list label {
  /* background: #ddd; */
  flex-grow: 1;
  line-height: 32px;
}
.app .todo_list li.completed label {
  color: #ccc;
  text-decoration: line-through;
}

.app .todo_list .remove_btn {
  height: 32px;
  border: 1px solid var(--primary-color);
  border-radius: 4px;
  padding: 0 5px;
  margin-left: 20px;
}
</style>
