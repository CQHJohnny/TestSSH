<script setup>
import { ref, onMounted, onUnmounted } from 'vue';
import { PAIN_POINTS } from '../constants';
import realNurse from '../assets/real.jpg';

const visibleMessages = ref(0);
const selectedIds = ref([]);
let timer = null;

onMounted(() => {
  timer = setInterval(() => {
    if (visibleMessages.value < 2) {
      visibleMessages.value++;
    }
  }, 1000);
});

onUnmounted(() => {
  if (timer) clearInterval(timer);
});

const toggleSelection = (id) => {
  if (selectedIds.value.includes(id)) {
    selectedIds.value = selectedIds.value.filter(i => i !== id);
  } else {
    selectedIds.value = [...selectedIds.value, id];
  }
};

const handleConfirm = () => {
  if (selectedIds.value.length > 0) {
    document.getElementById('cta')?.scrollIntoView({ behavior: 'smooth' });
  }
};
</script>

<template>
  <section class="py-24 px-6 bg-white w-full">
    <div class="page-container space-y-12">
      
      <!-- Section Header -->
      <div class="text-center mb-16">
        <span class="text-xs font-black text-blue-600 uppercase tracking-[0.3em] mb-4 block">Consulta Médica</span>
        <h2 class="text-4xl font-black text-slate-900">Diagnóstico Inicial 1 a 1</h2>
      </div>

      <!-- Nurse Intro Bubble 1 -->
      <div v-if="visibleMessages >= 1" class="flex items-start space-x-6 animate-fade-in transition-all duration-700">
        <div class="relative flex-shrink-0">
          <img 
            :src="realNurse" 
            alt="Enfermera Maria" 
            class="w-16 h-16 rounded-full border-4 border-white shadow-xl object-cover"
          />
          <div class="absolute bottom-0 right-0 w-4 h-4 bg-green-500 border-4 border-white rounded-full"></div>
        </div>
        <div class="bg-[#f1f5f9] p-7 rounded-[2.5rem] rounded-tl-none shadow-sm max-w-[85%] relative group">
          <p class="text-slate-800 text-lg md:text-xl leading-relaxed font-semibold">
            “¡Hola! Soy Maria, asesora de salud senior del equipo del Dr. Aaron. Antes de empezar, necesitamos entender las 'señales de auxilio' que envía su cuerpo.”
          </p>
          <div class="absolute top-0 -left-3 w-0 h-0 border-t-[12px] border-t-[#f1f5f9] border-l-[12px] border-l-transparent"></div>
          <span class="text-[10px] font-bold text-slate-400 absolute -bottom-6 left-0 uppercase tracking-widest">Maria • Asesora de Salud Senior</span>
        </div>
      </div>

      <div v-if="visibleMessages >= 2" class="flex items-start space-x-6 animate-fade-in transition-all duration-700 mt-12">
        <div class="relative flex-shrink-0">
          <img 
            :src="realNurse" 
            alt="Enfermera Maria" 
            class="w-16 h-16 rounded-full border-4 border-white shadow-xl object-cover"
          />
          <div class="absolute bottom-0 right-0 w-4 h-4 bg-green-500 border-4 border-white rounded-full"></div>
        </div>
        <div class="bg-[#f1f5f9] p-7 rounded-[2.5rem] rounded-tl-none shadow-sm max-w-[85%] relative">
          <p class="text-slate-800 text-lg md:text-xl leading-relaxed font-semibold">
            “¿Se ha encontrado con alguna de las siguientes situaciones en su camino de pérdida de peso? (Haga clic para elegir)”
          </p>
          <div class="absolute top-0 -left-3 w-0 h-0 border-t-[12px] border-t-[#f1f5f9] border-l-[12px] border-l-transparent"></div>
        </div>
      </div>

      <!-- Interactive Selection Area -->
      <div v-if="visibleMessages >= 2" class="pl-0 md:pl-24 space-y-6 animate-fade-in pt-8">
        <div class="grid md:grid-cols-2 gap-6">
          <button 
            v-for="point in PAIN_POINTS"
            :key="point.id"
            @click="toggleSelection(point.id)"
            :class="`w-full text-left p-8 rounded-[2.5rem] transition-all duration-500 border-2 flex flex-col items-start space-y-4 ${
              selectedIds.includes(point.id) 
              ? 'bg-blue-600 border-blue-600 shadow-2xl shadow-blue-100 scale-[1.03] -translate-y-2' 
              : 'bg-white border-slate-100 hover:border-blue-200 shadow-sm'
            }`"
          >
            <div :class="`p-4 rounded-2xl flex-shrink-0 transition-colors ${selectedIds.includes(point.id) ? 'bg-white/20 text-white' : 'bg-blue-50 text-blue-600'}`">
              <i :class="`${point.icon} text-2xl`"></i>
            </div>
            <div class="flex-1 min-w-0">
              <p :class="`font-semibold text-lg md:text-xl leading-relaxed mb-2 ${selectedIds.includes(point.id) ? 'text-white' : 'text-slate-900'}`">{{ point.title }}</p>
              <p :class="`text-sm md:text-base leading-relaxed ${selectedIds.includes(point.id) ? 'text-blue-50 font-medium' : 'text-slate-500'}`">{{ point.description }}</p>
            </div>
            <div :class="`self-end flex-shrink-0 w-8 h-8 rounded-full border-2 flex items-center justify-center transition-all ${selectedIds.includes(point.id) ? 'bg-white border-white' : 'border-slate-200'}`">
               <i v-if="selectedIds.includes(point.id)" class="fa-solid fa-check text-blue-600 text-sm"></i>
            </div>
          </button>
        </div>

        <div :class="`pt-12 transition-all duration-700 flex flex-col items-center ${selectedIds.length > 0 ? 'opacity-100 translate-y-0' : 'opacity-0 translate-y-8 pointer-events-none'}`">
           <button 
             @click="handleConfirm"
             class="w-full max-w-2xl bg-blue-600 hover:bg-blue-700 text-white py-6 rounded-[2rem] font-black shadow-2xl shadow-blue-200 flex items-center justify-center space-x-4 active:scale-95 transition-all group"
           >
             <span class="text-xl">Confirmar y obtener mi plan de reparación</span>
             <i class="fa-solid fa-arrow-right text-sm group-hover:translate-x-2 transition-transform"></i>
           </button>
           <div class="mt-6 flex items-center space-x-3 text-slate-400">
              <i class="fa-solid fa-shield-halved text-xs"></i>
              <p class="text-[10px] uppercase tracking-widest font-black">
                Sus selecciones son privadas y se usan solo para evaluación preliminar
              </p>
           </div>
        </div>
      </div>
    </div>
  </section>
</template>
