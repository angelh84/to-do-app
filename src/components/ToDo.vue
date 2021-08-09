<template>
  <div class="flex h-full flex-col justify-between">

    <!-- Header buttons -->
    <div class="flex fixed justify-between bottom-28 sm:top-0 sm:bottom-auto left-0 w-full p-6">
      <t-button 
        variant="text"
        :class="[
          'text-color text-sm text-red-500 hover:underline',
          'transition-opacity opacity-0 duration-700 pointer-events-none',
          {'opacity-100 pointer-events-auto': itemsChecked.length}
        ]"
        @click="deleteChecked"
      >
        Delete Selected
      </t-button>
      <t-button
        variant="secondary"
        :class="[
          'rounded text-sm',
          'transition-opacity opacity-0 duration-700 pointer-events-none',
          {'bg-yellow-100': showEditFields},
          {'opacity-100 pointer-events-auto': itemsChecked.length}
        ]"
        @click="editChecked"
      >
        Edit Selected
      </t-button>
    </div>

    <!-- Table -->
    <div :class="[
      'h-full w-full sm:w-600 mx-auto',
      'pt-10 sm:pt-32 pr-5 pb-3 pl-5 mb-44 sm:mb-36',
      'overflow-auto'
    ]">
      <div 
        :class="[
          {'border-b-2 border-gray-300': toDoList.length},
          'flex flex-column items-center p-1 pb-2'
        ]"
      >
        <t-checkbox
          v-if="toDoList.length"
          v-model="checkAll"
          class="mr-4"
          :value="true"
          @click="clickCheckAll(!checkAll)"
        />
        <h1 class="w-full">My to-do list</h1>
        <t-select
          v-if="toDoList.length"
          v-model="sort"
          placeholder="Sort"
          fixedClasses="border-0 block w-44 px-3 py-2 transition duration-100 ease-in-out focus:ring-2 focus:ring-blue-500 focus:outline-none focus:ring-opacity-50 disabled:opacity-50 disabled:cursor-not-allowed"
          :class="[
            'text-xs sm:text-sm',
            {'text-gray-400': sort === 'Sort' || sort === ''}
          ]"
          :options="[
            'Order (default)',
            'Life Changing',
            'Important',
            'Meh',
            'No Priority'
          ]"
        />
      </div>
      <template v-if="toDoList.length">
        <div 
          v-for="(item, index) in toDoList"
          :key="index"
          :class="[
            'flex flex-row items-center',
            'w-full',
            'h-14',
            'border-b'
          ]"
        >
          <label class="w-full flex items-center p-1 pr-4">
            <t-checkbox 
              v-model="item.checked"
              name="options"
              class="mr-4"
              :value="true"
            />
            <span 
              v-if="!showEditFields || !item.checked"
              class="break-all"
            >
              {{ item.name }}
            </span>
            <t-input
              v-else
              v-model="item.name"
              class="text-xs sm:text-sm"
              ref="listItem"
              placeholder="To-do item name"
            />
          </label>
          <div
            v-if="!showEditFields || !item.checked"
            :class="[
              labels(item.priority),
              'w-40 rounded p-2',
              'text-center text-white text-xs sm:text-sm'
            ]"
          >
            {{ item.priority }}
          </div>
          <t-select
            v-else
            v-model="item.priority"
            class="text-xs sm:text-sm"
            :options="[
              'No Priority',
              'Life Changing',
              'Important',
              'Meh'
            ]"
          />
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
          ref="toDoInput"
          v-model="toDoInput"
          placeholder="Add your next to-do"
          class="mr-4 w-full sm:h-16 sm:text-xl"
          value=""
          name="to-do"
          maxlength="100"
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
            {'text-gray-400': prioritySelect === ''}
          ]"
          :options="priorityOptions"
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
      prioritySelect: '',
      priorityOptions: [
        'Life Changing',
        'Important',
        'Meh'
      ],
      sort: 'Sort',
      checkAll: false,
      showEditFields: false
    }
  },
  watch: {
    itemsChecked (arr) {
      if (!arr.length) {
        this.showEditFields = false
      }
      
      if (arr.length === this.toDoList.length) {
        this.checkAll = true  
      } else {
        this.checkAll = false
      }

    },
    checkAll (bool) {
      if (bool) {
        this.toDoList.forEach(obj => obj.checked = true)
      }
    },
    sort (string) {
      this.sortByPriority(string)
    },
    toDoList: {
      deep: true,
      handler() {
        this.saveToDoList()
      }
    }
  },
  computed: {
    itemsChecked () {
      return this.toDoList.filter(obj => obj.checked)
    },
    itemsUnChecked () {
      return this.toDoList.filter(obj => !obj.checked)
    },
    sortByTimestamp () {
      return this.toDoList.slice().sort((a, b) => a.timeStamp - b.timeStamp)
    },
    labels () {
      return priority => {
        switch (priority.toLowerCase()) {
          case 'life changing':
            return 'bg-green-500'
          case 'important':
            return 'bg-yellow-500'
          case 'meh':
            return 'bg-blue-400'
          default:
            return 'border border-dashed rounded text-gray-400'
        }
      }
    }
  },
  methods: {
    addToDo () {
      this.toDoList.push({
        name: this.toDoInput,
        priority: this.prioritySelect ? this.prioritySelect : 'No Priority',
        checked: false,
        timeStamp: new Date()
      })
      this.clearFields()
      this.$refs.toDoInput.focus()
    },
    clearFields () {
      this.toDoInput = '',
      this.prioritySelect = ''
    },
    clickCheckAll(val) {
      // I needed to add this specific method even though
      // I am watching the 'checkAll' property because
      // I ONLY want to clear all checkboxes if I
      // unclick the actual master checkbox next to the <h1>
      // otherwise unclicking any other list item when 
      // 'all' are selected will deselect all.
      if (!val) {
        this.toDoList.forEach(obj => obj.checked = false)
      }
    },
    sortByPriority (sort) {
      const filterByPriority = this.toDoList.filter(obj => obj.priority === sort)
      const filterOutPriority = this.toDoList.filter(obj => obj.priority !== sort)
      const sortedArr = [...filterByPriority, ...filterOutPriority]

      this.toDoList = sort === '' || sort === 'Order (default)'
        ? this.sortByTimestamp
        : sortedArr
    },
    editChecked () {
      this.showEditFields = !this.showEditFields
      this.$nextTick(() => {
        if (this.showEditFields) {
          this.$refs.listItem[0].focus()
        }
      })
    },
    deleteChecked () {
      this.toDoList = this.itemsUnChecked
    },
    getToDoList () {
      return JSON.parse(localStorage.getItem('toDoList'))
    },
    saveToDoList () {
      localStorage.setItem('toDoList', JSON.stringify(this.toDoList))
    }
  },
  created () {
    // Load toDoList from localStorage if localStorage is 
    // available and the toDoList property has length
    const localStorageList = this.getToDoList()
    if (localStorage && localStorageList && localStorageList.length) {
      this.toDoList = localStorageList
    }
  },
  mounted () {
    // Focus the 'add to-do item' input when app loads.
    this.$refs.toDoInput.focus()
  }
}
</script>
