<template>
  <div class="flex justify-center p-4 sm:p-6">
    <div
      id="chat"
      class="w-full max-w-6xl h-[80vh] bg-gray-100 rounded-xl p-4 sm:p-6 overflow-y-auto shadow-md flex flex-col"
    >
      <div v-for="(msg, i) in messages" :key="i" class="mb-4 flex flex-col">
        <div
          :class="[
            'p-3 rounded-xl break-words max-w-[80%]',
            msg.sender === 'bot'
              ? 'bg-gray-200 self-start'
              : 'bg-blue-600 text-white self-end ml-auto'
          ]"
        >
          {{ msg.text }}
        </div>

        <!-- Opciones como botones -->
        <div v-if="msg.options && !finished" class="flex flex-col mt-2 space-y-2">
          <button
            v-for="(opt, j) in msg.options"
            :key="j"
            @click="handleAnswer(opt)"
            class="bg-blue-600 text-white px-4 py-2 rounded-lg hover:bg-blue-700 transition-colors"
          >
            {{ opt.title }}
          </button>
        </div>

        <!-- Input para nombre -->
        <div v-if="msg.input && !finished" class="mt-2 flex justify-end ">
          <input
            v-model="userNameInput"
            @keyup.enter="submitName()"
            placeholder="Type your name..."
            class="border rounded px-3 py-2 w-48 mr-2"
          />
          <button
            @click="submitName"
            class="mt-2 bg-blue-600 text-white px-4 py-2 rounded-lg hover:bg-blue-700 transition-colors"
          >
            Submit
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, nextTick, onMounted } from "vue";
import questions from "../data/quiz.js";

const messages = ref([
  { sender: "bot", text: "Welcome to the Sorting Hat test! 🏰" },
]);

const userNameInput = ref("");
const userName = ref("");
const currentQuestionIndex = ref(0);
const scores = ref({ gryffindor: 0, ravenclaw: 0, hufflepuff: 0, slytherin: 0 });
const finished = ref(false);

// Inicial: pedir nombre
onMounted(() => {
  messages.value.push({
    sender: "bot",
    text: "What's your name?",
    input: true,
  });
  scrollToBottom();
});

function submitName() {
  if (!userNameInput.value.trim()) return;

  userName.value = userNameInput.value.trim();
  messages.value.push({ sender: "user", text: userName.value });

  // Quitar el input del mensaje del bot
  const lastBotIndex = messages.value
    .slice()
    .reverse()
    .findIndex((msg) => msg.sender === "bot" && msg.input);
  if (lastBotIndex !== -1) {
    const realIndex = messages.value.length - 1 - lastBotIndex;
    messages.value[realIndex].input = null;
  }

  // Mostrar primera pregunta del quiz
  if (questions.length > 0) {
    const firstQ = questions[0];
    messages.value.push({
      sender: "bot",
      text: `Hi ${userName.value}! Let's start the Sorting Hat quiz. ${firstQ.title}`,
      options: firstQ.answers,
    });
  }

  scrollToBottom();
}


function handleAnswer(answer) {
  if (finished.value) return;

  // Guardar la respuesta del usuario
  messages.value.push({ sender: "user", text: answer.title });

  // Actualizar puntajes
  for (let key in answer.scores) scores.value[key] += answer.scores[key];

  // Quitar las opciones del mensaje anterior
  const lastBotIndex = messages.value
    .slice()
    .reverse()
    .findIndex((msg) => msg.sender === "bot" && msg.options);
  if (lastBotIndex !== -1) {
    // Como usamos reverse, calculamos el índice real
    const realIndex = messages.value.length - 1 - lastBotIndex;
    messages.value[realIndex].options = null;
  }

  // Pasar a la siguiente pregunta
  if (currentQuestionIndex.value < questions.length - 1) {
    currentQuestionIndex.value++;
    const nextQ = questions[currentQuestionIndex.value];
    setTimeout(() => {
      messages.value.push({
        sender: "bot",
        text: nextQ.title,
        options: nextQ.answers,
      });
      scrollToBottom();
    }, 600);
  } else {
    finished.value = true;
    setTimeout(() => {
      messages.value.push({ sender: "bot", text: `Test completed! 🥳` });
      messages.value.push({
        sender: "bot",
        text: `Your house is: ${result.value.toUpperCase()} 🏆`,
      });
      scrollToBottom();
    }, 600);
  }
}


const result = computed(() => {
  let maxScore = -Infinity, house = null;
  for (let key in scores.value) {
    if (scores.value[key] > maxScore) {
      maxScore = scores.value[key];
      house = key;
    }
  }
  return house;
});

function scrollToBottom() {
  nextTick(() => {
    const chat = document.getElementById("chat");
    if (chat) chat.scrollTo({ top: chat.scrollHeight, behavior: "smooth" });
  });
}
</script>
