<template>
  <!-- layout start -->
  <main class="mt-5 flex w-screen items-center justify-center text-base font-medium text-gray-700">
    <!-- todo list start -->
    <section
      class="base-style my-2 flex w-11/12 flex-col border border-slate-100 bg-slate-50 p-3 md:my-10 md:w-8/12 md:p-5"
    >
      <!-- title -->
      <div class="my-2 text-3xl font-bold">My List</div>

      <!-- input -->
      <div class="flex flex-row py-2">
        <input
          v-model="title"
          type="text"
          class="input"
          placeholder="Input Your TodoList"
          @keyup.enter="handleAddList"
        />

        <button
          type="button"
          class="base-style ml-2 bg-slate-300 px-5 text-white hover:bg-slate-400 focus:bg-slate-400 active:bg-slate-400 md:ml-5"
          @click="handleAddList"
        >
          ADD
        </button>
      </div>

      <!-- information -->
      <div class="mb-3 flex flex-row">
        <div class="text-sm text-gray-400">Total: {{ list.length }}</div>
      </div>

      <!-- list start-->
      <ul>
        <li
          v-for="(item, index) of list"
          :key="item.id"
          class="flex flex-row items-center justify-between border-b border-slate-200 py-3 last:border-none"
        >
          <div class="flex flex-row items-center">
            <!-- update btn -->
            <button
              class="base-style bg-slate-300 p-2 text-white hover:bg-slate-400 focus:bg-slate-400 active:bg-slate-400"
              @click="handleUpdateShow(index)"
            >
              <svg
                xmlns="http://www.w3.org/2000/svg"
                fill="none"
                viewBox="0 0 24 24"
                stroke-width="2"
                stroke="currentColor"
                class="h-5 w-5"
              >
                <path
                  stroke-linecap="round"
                  stroke-linejoin="round"
                  d="M16.862 4.487l1.687-1.688a1.875 1.875 0 112.652 2.652L6.832 19.82a4.5 4.5 0 01-1.897 1.13l-2.685.8.8-2.685a4.5 4.5 0 011.13-1.897L16.863 4.487zm0 0L19.5 7.125"
                />
              </svg>
            </button>

            <!-- list text/input -->
            <div class="mx-2 text-lg font-extrabold md:mx-4">
              <input
                v-model="item.title"
                type="text"
                class="w-full bg-slate-50"
                :class="item.isDone ? 'line-through' : item.isUpdate ? 'input' : ''"
                :disabled="!item.isUpdate || item.isDone"
                @keyup.enter="handleUpdateList($event, index)"
                @blur="handleUpdateShow(index)"
              />
            </div>
          </div>

          <!-- remove btn & change status btn -->
          <div class="flex flex-row items-center">
            <button
              class="base-style mr-2 p-2 text-white"
              :class="item.isDone ? 'btn_notdone' : 'btn_done'"
              @click="handleChangeStatus(index, item.isDone ? false : true)"
            >
              {{ item.isDone ? 'Reset' : 'Done' }}
            </button>

            <button
              class="base-style bg-rose-300 p-2 text-white hover:bg-rose-400 focus:bg-rose-400 active:bg-rose-400"
              @click="handleRemoveList(index)"
            >
              <svg
                xmlns="http://www.w3.org/2000/svg"
                fill="none"
                viewBox="0 0 24 24"
                stroke-width="1.5"
                stroke="currentColor"
                class="h-6 w-6"
              >
                <path
                  stroke-linecap="round"
                  stroke-linejoin="round"
                  d="M14.74 9l-.346 9m-4.788 0L9.26 9m9.968-3.21c.342.052.682.107 1.022.166m-1.022-.165L18.16 19.673a2.25 2.25 0 01-2.244 2.077H8.084a2.25 2.25 0 01-2.244-2.077L4.772 5.79m14.456 0a48.108 48.108 0 00-3.478-.397m-12 .562c.34-.059.68-.114 1.022-.165m0 0a48.11 48.11 0 013.478-.397m7.5 0v-.916c0-1.18-.91-2.164-2.09-2.201a51.964 51.964 0 00-3.32 0c-1.18.037-2.09 1.022-2.09 2.201v.916m7.5 0a48.667 48.667 0 00-7.5 0"
                />
              </svg>
            </button>
          </div>
        </li>
        <!-- list end -->
      </ul>

      <!-- footer -->
      <div class="mt-3 mb-2 text-center text-xs text-gray-400">
        <a href="https://github.com/kensoz/my-todo-list" target="_blank">GitHub</a>
      </div>
      <!-- todo list end -->
    </section>
    <!-- layout end -->
  </main>
</template>

<script setup lang="ts">
// ----- Import ------
import { reactive, ref } from 'vue'
import type { IList } from './types'

// ----- Variables ------
const title = ref<string>('')
const list = reactive<Array<IList>>([])

// ----- Functions ------
// 1.Add
const handleAddList = (): void => {
  // Invalid
  if (title.value === '') return alert('Value is Invalid')

  // Already Exist
  if (list.find((item: IList): boolean => item.title === title.value)) {
    return alert('Value is Already Exist')
  }

  // Add
  list.push({
    id: String(Date.now()),
    title: title.value.trim(),
    isDone: false,
    isUpdate: false,
  })

  title.value = ''
}

// 2.Update
const handleUpdateShow = (index: number): void => {
  list[index].isUpdate = !list[index].isUpdate
}

const handleUpdateList = (e: KeyboardEvent, index: number): void => {
  const button: HTMLButtonElement = e.target as HTMLButtonElement
  list[index].title = button.value.trim()
}

// 3.Remove
const handleRemoveList = (index: number): void => {
  list.splice(index, 1)
}

// 4.Change Status
const handleChangeStatus = (index: number, status: boolean): void => {
  list[index].isDone = status
}
</script>

<style lang="postcss" scoped>
.base-style {
  @apply rounded-md shadow-sm;
}

.btn_done {
  @apply bg-blue-300  hover:bg-blue-400 focus:bg-blue-400 active:bg-blue-400;
}

.btn_notdone {
  @apply bg-green-300  hover:bg-green-400 focus:bg-green-400 active:bg-green-400;
}

.input {
  @apply base-style w-full border border-slate-100 bg-white p-2 focus:border-slate-300;
}
</style>
