<template>
  <div class="m-20">
    <h1 class="my-10 text-3xl text-center font-bold text-gray-900">
      Submit your interview
    </h1>
    <div class="my-5" v-for="(question,index) in questions" :key="index">
      <h2 class=" leading-6 text-gray-600">{{index+1}}. {{question.question}}</h2>
      <div class="mx-10">
        <video v-if="!question.recording && question.answer" class="w-96 h-56 my-5 bg-gray-700" :src="question.answer" controls></video>
        <video v-else-if="question.recording"  class="w-96 h-56 my-5 bg-gray-700" :id="`player-${index}`" controls="false"></video>
        <button v-if="!question.recording" @click="recordAnswer(index)" type="button" class="my-5 inline-block items-center px-2.5 py-1.5 border border-transparent text-xs font-medium rounded text-indigo-700 bg-indigo-100 hover:bg-indigo-200 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500">
          {{question.answer ? 'Record again' : 'Record answer'}}
        </button>
        <button v-if="question.recording" @click="questions[index].recorder.stop()" type="button" class="my-5 inline-block items-center px-2.5 py-1.5 border border-transparent text-xs font-medium rounded text-indigo-700 bg-indigo-100 hover:bg-indigo-200 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500">
          Stop Recording
        </button>
      </div>
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
  data(){
    return {
      questions:[
        {
          question: "Tell me something about yourself.",
          recording:false,
          recorder:null,
          recordedChunks:[],
          answer:null
        },
        {
          question: "How did you hear about this position?",
          recording:false,
          recorder:null,
          recordedChunks:[],
          answer:null
        },
        {
          question: "Why do you want to work here?",
          recording:false,
          recorder:null,
          recordedChunks:[],
          answer:null
        },
      ]
    }
  },
  methods:{
    recordAnswer(index){
      this.questions[index].recording = true;
      navigator.mediaDevices.getUserMedia({ audio: true, video: true }).then(
        stream => this.handleRecordingSuccess(index,stream) 
      );
    },
    handleRecordingSuccess(index,stream){
      this.questions[index].recorder = new MediaRecorder(stream);
      document.getElementById(`player-${index}`).srcObject = stream;
      document.getElementById(`player-${index}`).play();
      this.questions[index].recorder.addEventListener(
        'dataavailable',
        e => this.handleDataAvailable(index,e)
      );

      this.questions[index].recorder.addEventListener(
        'stop', 
        () => this.handleRecordingStopped(index,stream)
      );

      this.questions[index].recorder.start();
    },
    handleDataAvailable(index, e){
        if (e.data.size > 0) {
          this.questions[index].recordedChunks.push(e.data);
        }
    },
    handleRecordingStopped(index,stream){
        stream.getTracks().forEach(track => track.stop());
        this.questions[index].recording = false;
        document.getElementById(`player-${index}`).pause();
        document.getElementById(`player-${index}`).srcObject = null;
        this.questions[index].answer = URL.createObjectURL(new Blob(this.questions[index].recordedChunks), {type: 'video/mp4'});
    },
  }
}
</script>
