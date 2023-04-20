<script setup>
import { reactive, ref, computed, watch } from "vue";

const arr = reactive([]);
let index = 0;

const sortedArr = computed(() => {
  return arr.sort((a, b) => b.index - a.index);
});

const text = ref("");
let updatedText;

function create() {
  if (text.value) {
    const newText = text.value;
    const date = new Date();
    const day = date.getDate();
    const month = date.getMonth();
    const year = date.getFullYear();
    const monthList = [
      "January",
      "February",
      "March",
      "April",
      "May",
      "June",
      "July",
      "August",
      "September",
      "October",
      "November",
      "December",
    ];
    arr.push({
      index: index++,
      text: newText,
      date: `${day} ${monthList[month]} ${year}`,
    });
    text.value = "";
  }
}

function del(i) {
  const position = arr.findIndex((e) => e.index === i);
  arr.splice(position, 1);
}

function update(i) {
  const position = arr.findIndex((e) => e.index === i);
  if (updatedText) {
    arr[position].text = updatedText;
    updatedText = "";
  }
}
</script>

<template>
  <div class="main bg-gray-100">
    <div class="main__input">
      <input
        v-model="text"
        placeholder="Write something here"
        @keyup.enter="create"
        class="main__input--input border-b"
      />
      <button @click="create" class="main__input--btn">Create</button>
    </div>

    <div class="main__output pb-4">
      <pre
        v-if="arr.length > 0"
        class="pt-6 whitespace-pre-wrap overflow-hidden overflow-ellipsis"
        >{{ arr }}</pre
      >

      <div v-for="e in sortedArr">
        <div
          class="flex flex-col sm:flex-row items-center mt-6 p-10 border-b rounded-lg bg-white gap-4"
          :tabindex="0"
          :index="e.index"
        >
          <div class="flex flex-col gap-4 w-full">
            <div>
              {{ e.index + " - " + e.date }}
            </div>
            <textarea
              class="h-[60px] border rounded-lg p-4"
              :value="e.text"
              @input="(e) => (updatedText = e.target.value)"
            ></textarea>
          </div>
          <div class="flex sm:flex-col gap-4 w-full sm:w-fit">
            <button @click="update(e.index)" class="btn">Update</button>
            <button @click="del(e.index)" class="btn">Delete</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped lang="scss">
.main {
  height: calc(100vh - 100px);
  overflow-y: scroll;

  padding: 0 1.5rem;

  @media (min-width: 1280px) {
    padding: 0 16rem;
  }

  &__input {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 2.5rem;

    padding-top: 1.5rem;

    &--input {
      width: 100%;

      border-radius: 0.5rem;
      padding: 1rem;

      &:focus {
        outline: none;
      }

      &::placeholder {
        color: black;
      }
    }

    &--btn {
      width: fit-content;
      border: 1px solid black;
      background-color: black;
      border-radius: 0.5rem;
      padding: 0.5rem 1rem;
      color: white;
      &:hover {
        background-color: #2a2a2a;
      }
      &:focus {
        outline: 2px solid black;
        border: 2px solid white;
      }
    }
  }
}
.btn {
  width: 100%;
  border: 1px solid lightgray;
  border-radius: 0.5rem;
  padding: 0.5rem 1rem;
  &:hover {
    background-color: #d3d3d350;
    border-color: #d3d3d350;
  }
}
</style>
