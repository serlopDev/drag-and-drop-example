<script setup lang="ts">
import { useDragAndDrop } from "@formkit/drag-and-drop/vue";
import { onMounted, ref, reactive } from "vue";

const plan = ref({
  days: [
    {
      name: "Day 1",
      todos: [],
    },
    {
      name: "Day 2",
      todos: [],
    },
  ],
});

const todoItems = [
  "Make Test",
  "Make Controllers",
  "Deploy Project",
  "Todo Task 1",
  "Todo Task 2",
  "Todo Task 3",
  "Todo Task 4",
  "Todo Task 5",
  "Todo Task 6",
  "Todo Task 7",
  "Todo Task 8",
  "Todo Task 9",
  "Todo Task 10",
];
const doneItems = reactive([]);

onMounted(async () => {
  todoItems;
});

const [todoList, todos] = useDragAndDrop(todoItems, {
  group: "todoList",
  sortable: false,
  selectedClass: "bg-blue-200 text-white",
  draggingClass: "bg-blue-200 text-white", // ! Doesn't work when enter on DropZone
  handleEnd: (event) => {
    // console.log('Element dragged:', event)
  },
});

const [doneList, dones] = useDragAndDrop(doneItems, {
  group: "doneList",
  sortable: false,
  disabled: true,
  handleEnd: (event) => {
    const draggedItem = event.draggedNode?.data;
    if (draggedItem) {
      const container = event.currentParent?.el;
      console.log(event);

      console.log(container);

      if (container) {
        console.log("Element droped:", draggedItem.value);
        doneItems.push(draggedItem.value);
      }
    }
  },
});
const addDay = () => {
  plan.value.days.push({
    name: `Day ${plan.value.days.length + 1}`,
    todos: [],
  });
};
</script>

<template>
  <div class="py-10 px-20">
    <div class="flex items-center justify-end">
      <button
        class="bg-blue-500/40 rounded-full px-3 py-1 mb-10"
        @click="addDay"
      >
        Add day
      </button>
    </div>
    <div class="grid gap-10 grid-cols-2 items-start">
      <div
        class="max-h-[650px] overflow-y-auto rounded-xl p-8 border-2 border-dashed"
      >
        <ul ref="todoList">
          <li
            v-for="todo in todos"
            :key="todo"
            class="p-4 border bg-gray-200 border-gray-300 rounded cursor-grab mb-2 last:mb-0"
          >
            {{ todo }}
          </li>
        </ul>
      </div>
      <ul
        ref="doneList"
        class="p-8 border-2 border-dashed min-h-[150px] rounded-lg space-y-3"
      >
        <li
          v-for="day in plan?.days"
          :key="day.name"
          class="flex flex-col p-4 bg-gray-200 border border-gray-300 rounded"
        >
          <span> {{ day.name }}</span>
          <h1 class="font-bold">TODO'S</h1>
          <ul
            class="p-8 border-2 border-dashed min-h-[150px] rounded-lg space-y-3"
          >
            <li
              v-for="dayTodo in day.todos"
              :key="dayTodo"
              class="p-4 bg-gray-200 border border-gray-300 rounded"
            >
              <span> {{ dayTodo }}</span>
            </li>
          </ul>
        </li>
      </ul>
    </div>
  </div>
</template>
