<template>
  <div class="flex h-full flex-col justify-between pb-96">
    <div class="flex flex-col h-full w-full sm:w-600 mx-auto justify-start items-center">
      <div 
        v-for="(item, index) in 100"
        :key="index"
        :class="[
          'flex flex-row',
          'w-full',
          'border-t',
          {'border-b': index === 99}
        ]"
      >
        <label class="w-full">
          <t-checkbox 
            name="options"
            :value="true"
          />
          <span class="">Hello World {{ index }}</span>
        </label>
        <div class="w-40 text-center">
          Life Changing
        </div>
        <div class="w-40 text-center">
          
        </div>
      </div>
    </div>
    <div class="p-6 w-full sm:pl-12 sm:pr-12 bg-gray-700 fixed bottom-0 flex flex-col sm:flex-row">
      <div class="flex flex-row mb-4 w-full">
        <t-input
          id="to-do-input"
          v-model="toDo"
          placeholder="Add your next to-do"
          class="mr-4 w-full sm:h-16 sm:text-xl"
          fixedClasses="border-0"
          value=""
          name="to-do"
        />
        <t-select
          id="to-do-priority"
          v-model="priority"
          name="priority"
          placeholder="Priority"
          fixedClasses="border-0"
          :class="[
            'w-42',
            'hidden sm:block',
            'mr-8',
            'sm:text-xl',
            {'text-gray-400': priority === ''}
          ]"
          :options="[
            'Life Changing',
            'Important',
            'Meh'
          ]"
        />
        <button
          type="button"
          class="w-8 sm:w-12"
          @click="addToDo"
        >
          <svg 
            class="fill-current text-blue-400 hover:text-gray-50"
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
            v-model="priority"
            name="priority" 
            value="Life Changing"
          />
          <span class="ml-2">Life Changing</span>
        </label>
        <label class="flex items-center mr-4 text-sm text-gray-50">
          <t-radio 
            v-model="priority"
            name="priority" 
            value="Important"
          />
          <span class="ml-2">Important</span>
        </label>
        <label class="flex items-center mr-4 text-sm text-gray-50">
          <t-radio 
            v-model="priority"
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
      toDo: '',
      priority: ''
    }
  },
  methods: {
    addToDo () {
      console.log('click')
    }
  }
}
</script>
