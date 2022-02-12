<template>
  <div class="m-20">
    <h1 class="my-10 text-3xl text-center font-bold text-gray-900">
      Submit your interview
    </h1>
    <div class="my-5" v-for="(question,index) in questions" :key="index">
      <h2 class=" leading-6 text-gray-600">{{index+1}}. {{question.question}}</h2>
    </div>
    <div class="my-5 text-right">
      <button type="button" class="inline-flex items-center px-4 py-2 border border-transparent text-sm font-medium rounded-md shadow-sm text-white bg-indigo-600 hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500">
        Submit interview
      </button>
    </div>
  </div>

</template>

<script>

export default {
  name: 'IndexPage',
  data(){
    return {
      questions:[
        {
          question: "Tell me something about yourself."
        },
        {
          question: "How did you hear about this position?"
        },
        {
          question: "Why do you want to work here?"
        },
      ]
    }
  }
}
</script>
