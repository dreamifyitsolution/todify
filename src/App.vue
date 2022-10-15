<script setup>
import { reactive, ref, watch } from "vue";
import uniqid from "uniqid";

const defaultData = {
  columns: [
    {
      name: "now",
      list: [],
    },
    {
      name: "next",
      list: [],
    },
    {
      name: "later",
      list: [],
    },
  ],
};
const localStorageString = localStorage.getItem("todify-app-data");
const localStorageData = localStorageString
  ? JSON.parse(localStorageString)
  : defaultData;
const data = reactive(localStorageData);

watch(data, (newData) => {
  localStorage.setItem("todify-app-data", JSON.stringify(newData));
});

function addItem(event, column) {
  const formData = new FormData(event.target);
  const description = formData.get("description");
  console.log(description);

  column.list.push({
    id: uniqid(),
    description,
    state: "new",
  });

  event.target.reset();
}
</script>

<template>
  <div class="flex flex-row justify-center space-x-6 pt-10">
    <div
      v-for="column of data.columns"
      :key="column.name"
      class="rounded-md shadow-md bg-slate-100 py-4 px-4 w-1/4 space-y-4"
    >
      <div>{{ column.name }}</div>
      <div
        v-for="item of column.list"
        :key="item.id"
        class="bg-slate-50 rounded-md py-2 px-3 shadow-md"
      >
        {{ item.description }}
      </div>
      <div class="flex flex-col items-end">
        <form @submit.prevent="addItem($event, column)">
          <input
            type="text"
            name="description"
            class="w-full border px-3 py-2 rounded-md mb-2 shadow-sm"
            autocomplete="off"
          />
          <button>Add</button>
        </form>
      </div>
    </div>
  </div>
</template>
