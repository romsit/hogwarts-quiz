<template>
  <div class="flex justify-center p-4 sm:p-6">
    <div id="chat" class="w-full max-w-6xl h-[80vh] bg-gray-100 rounded-xl p-4 sm:p-6 overflow-y-auto shadow-md">
      <!-- Transición en grupo para mensajes -->
      <transition-group name="message" tag="div" class="flex flex-col">
        <div v-for="(msg, i) in messages" :key="msg.id" class="mb-4 flex flex-col">

          <!-- Mensaje del usuario -->
          <div v-if="msg.sender === 'user'"
            class="p-3 rounded-xl break-words max-w-[80%] bg-blue-600 text-white self-end ml-auto">
            {{ msg.text }}
          </div>

          <!-- Mensaje del bot -->
          <div v-else-if="msg.sender === 'bot'" class="p-3 rounded-xl break-words max-w-[80%] bg-gray-200 self-start">
            {{ msg.text }}
          </div>

          <!-- Bloque de opciones del bot, animado con <transition> -->
          <transition name="fade">
            <div v-if="msg.options && !finished" class="flex flex-col mt-2 space-y-2">
              <button v-for="(opt, j) in msg.options" :key="j" @click="handleAnswer(opt, i)"
                class="bg-blue-600 text-white px-4 py-2 rounded-lg hover:bg-blue-700 transition-colors">
                {{ opt.title }}
              </button>
            </div>
          </transition>

          <!-- Input para nombre -->
          <div v-if="msg.input && !finished" class="mt-2 flex justify-end">
            <input v-model="userNameInput" @keyup.enter="submitName" placeholder="Type your name..."
              class="border rounded px-3 py-2 w-48 mr-2" />
            <button @click="submitName"
              class="bg-blue-600 text-white px-4 py-2 rounded-lg hover:bg-blue-700 transition-colors">
              Submit
            </button>
          </div>
        </div>
      </transition-group>
      <!-- Botón para rehacer el test -->
      <div v-if="finished" class="mt-4 flex justify-center">
        <button
          @click="resetQuiz"
          class="bg-blue-600 text-white px-4 py-2 rounded-lg hover:bg-blue-700 transition-colors"
        >
          Retake the test
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, nextTick, onMounted } from "vue";
import questions from "../data/quiz.js";

const messages = ref([
  { id: crypto.randomUUID(), sender: "bot", text: "Welcome to the Sorting Hat test! 🏰" },
]);
const userNameInput = ref("");
const userName = ref("");
const currentQuestionIndex = ref(0);
const scores = ref({ gryffindor: 0, ravenclaw: 0, hufflepuff: 0, slytherin: 0 });
const finished = ref(false);

// Inicializa la conversación (mensaje de bienvenida y petición de nombre)
function initConversation() {
  messages.value = [
    { id: crypto.randomUUID(), sender: "bot", text: "Welcome to the Sorting Hat test! 🏰" },
    {
      id: crypto.randomUUID(),
      sender: "bot",
      text: "What's your name?",
      input: true,
    },
  ];
}

// Llama a initConversation al montar el componente
onMounted(() => initConversation());

// Define resetQuiz para reiniciar todas las variables y mostrar de nuevo la conversación inicial
function resetQuiz() {
  userNameInput.value = "";
  userName.value = "";
  currentQuestionIndex.value = 0;
  scores.value = { gryffindor: 0, ravenclaw: 0, hufflepuff: 0, slytherin: 0 };
  finished.value = false;
  initConversation();
}

function submitName() {
  const name = userNameInput.value.trim();
  if (!name) return;

  userName.value = name;
  userNameInput.value = "";
  messages.value.push({ id: crypto.randomUUID(), sender: "user", text: userName.value });

  // Eliminar el input
  const index = messages.value.findIndex(msg => msg.input);
  if (index !== -1) {
    messages.value[index].input = null;
  }

  // Primera pregunta
  const firstQ = questions[0];
  if (firstQ) {
    messages.value.push({
      id: crypto.randomUUID(),
      sender: "bot",
      text: `Hi ${userName.value}! Let's start the Sorting Hat quiz. ${firstQ.title}`,
      options: firstQ.answers,
    });
  }

  scrollToBottom();
}

function handleAnswer(answer, msgIndex) {
  if (finished.value) return;

  // Ocultar opciones con una pequeña pausa para que la animación se perciba mejor
  messages.value[msgIndex].options = null;

  // Mostrar respuesta de usuario
  messages.value.push({ id: crypto.randomUUID(), sender: "user", text: answer.title });

  // Actualizar puntuaciones
  for (let key in answer.scores) {
    scores.value[key] += answer.scores[key];
  }

  // Mostrar siguiente pregunta o finalizar
  if (currentQuestionIndex.value < questions.length - 1) {
    currentQuestionIndex.value++;
    const nextQ = questions[currentQuestionIndex.value];
    setTimeout(() => {
      messages.value.push({
        id: crypto.randomUUID(),
        sender: "bot",
        text: nextQ.title,
        options: nextQ.answers,
      });
      scrollToBottom();
    }, 200);
  } else {
    finished.value = true;
    setTimeout(() => {
      messages.value.push({
        id: crypto.randomUUID(),
        sender: "bot",
        text: "Test completed! 🥳",
      });
      messages.value.push({
        id: crypto.randomUUID(),
        sender: "bot",
        text: `Your house is: ${capitalize(result.value)} 🏆`,
      });
      scrollToBottom();
    }, 400);
  }
}

const result = computed(() => {
  let maxScore = -Infinity;
  let house = "";
  for (let key in scores.value) {
    if (scores.value[key] > maxScore) {
      maxScore = scores.value[key];
      house = key;
    }
  }
  return house;
});

function capitalize(word) {
  return word.charAt(0).toUpperCase() + word.slice(1);
}

function scrollToBottom() {
  nextTick(() => {
    const chat = document.getElementById("chat");
    if (chat) {
      chat.scrollTo({ top: chat.scrollHeight, behavior: "smooth" });
    }
  });
}
</script>

<style scoped>
/* Mensajes: efecto deslizamiento y desvanecido */
.message-enter-active,
.message-leave-active {
  transition: all 0.4s ease;
}

.message-enter-from {
  opacity: 0;
  transform: translateY(8px);
}

.message-leave-to {
  opacity: 0;
  transform: translateY(-8px);
}

/* Opciones: fade-in/fade-out */
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.4s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>
