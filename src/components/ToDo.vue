<template>
  <div class="flex h-full flex-col justify-between">
    <!-- Header button -->
    <div class="flex fixed justify-between top-0 left-0 w-full p-6">
      <t-button 
        :class="[
          'text-color text-sm text-red-500 hover:underline',
          'transition-opacity opacity-0 duration-700',
          {'opacity-100': itemsChecked.length}
        ]"
        variant="text"
      >
        Delete Selected
      </t-button>
      <t-button
        class="rounded text-sm transition-opacity opacity-0 duration-700"
        :class="{'opacity-100': itemsChecked.length}"
        variant="secondary"
      >
        Edit Selected
      </t-button>
    </div>

    <!-- Table -->
    <div :class="[
      'h-full w-full sm:w-600 mx-auto',
      'pt-32 pr-5 pb-3 pl-5',
      'overflow-auto'
    ]">
      <h1 class="mb-4">My to-do list</h1>
      <template v-if="toDoList.length">
        <div 
          v-for="(item, index) in toDoList"
          :key="index"
          :class="[
            'flex flex-row',
            'w-full',
            'p-3',
            'border-t',
            {'border-b': index === toDoList.length - 1}
          ]"
        >
          <label class="w-full flex items-center">
            <t-checkbox 
              v-model="item.checked"
              name="options"
              class="mr-4"
              :value="true"
            />
            <span class="">{{ item.name }}</span>
          </label>
          <div class="w-40 text-center">
            {{ item.priority }}
          </div>
        </div>
      </template>
      <template v-else>
        <p :class="[
          'border-2 border-dashed',
          'rounded text-gray-400 p-3'
        ]">
          Add your first to-do item below.
        </p>
      </template>
    </div>

    <!-- Footer fields -->
    <div :class="[
      'fixed bottom-0',
      'flex flex-col sm:flex-row',
      'p-6 sm:pr-12 sm:pl-12', 
      'bg-gray-700',
      'w-full'
    ]">
      <div class="flex flex-row mb-4 w-full">
        <t-input
          id="to-do-input"
          v-model="toDoInput"
          placeholder="Add your next to-do"
          class="mr-4 w-full sm:h-16 sm:text-xl rounded"
          value=""
          name="to-do"
        />
        <t-select
          id="to-do-priority"
          v-model="prioritySelect"
          name="priority"
          placeholder="Priority"
          :class="[
            'w-42',
            'hidden sm:block',
            'mr-8',
            'sm:text-xl',
            'rounded',
            {'text-gray-400': prioritySelect === ''}
          ]"
          :options="[
            'Life Changing',
            'Important',
            'Meh'
          ]"
        />
        <button
          type="button"
          :disabled="!toDoInput.length"
          :class="{'pointer-events-none': !toDoInput.length}"
          class="w-8 sm:w-12 disabled:opacity-50"
          @click="addToDo"
        >
          <svg 
            class="fill-current text-green-500 hover:text-green-300"
            xmlns="http://www.w3.org/2000/svg" 
            viewBox="0 0 24 24"
          >
            <path d="M24 10h-10v-10h-4v10h-10v4h10v10h4v-10h10z"/>
          </svg>
        </button>
      </div>
      <div class="flex flex-row sm:hidden">
        <label class="flex items-center mr-4 text-sm text-gray-50">
          <t-radio 
            v-model="prioritySelect"
            name="priority" 
            value="Life Changing"
          />
          <span class="ml-2">Life Changing</span>
        </label>
        <label class="flex items-center mr-4 text-sm text-gray-50">
          <t-radio 
            v-model="prioritySelect"
            name="priority" 
            value="Important"
          />
          <span class="ml-2">Important</span>
        </label>
        <label class="flex items-center mr-4 text-sm text-gray-50">
          <t-radio 
            v-model="prioritySelect"
            name="priority" 
            value="Meh"
          />
          <span class="ml-2">Meh</span>
        </label>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ToDo',
  props: {
  },
  data () {
    return {
      toDoList: [],
      toDoInput: '',
      prioritySelect: ''
    }
  },
  computed: {
    itemsChecked () {
      return this.toDoList.filter(obj => obj.checked)
    }
  },
  methods: {
    addToDo () {
      this.toDoList.push({
        name: this.toDoInput,
        priority: this.prioritySelect,
        checked: false,
        timeStamp: new Date()
      })
      this.clearFields()
    },
    clearFields () {
      this.toDoInput = '',
      this.prioritySelect = ''
    }
  }
}
</script>
