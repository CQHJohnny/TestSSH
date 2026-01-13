<script setup>
import { ref, onMounted, onUnmounted } from 'vue';
import ChatInterface from './components/ChatInterface.vue';
import { REVIEWS } from './constants';
import logo from './assets/logo.png';
import topImg from './assets/top.png';
import person1 from './assets/rz/person/1.png';
import person2 from './assets/rz/person/2.png';
import person3 from './assets/rz/person/3.png';
import nbeBadge from './assets/nbe.png';

const timeLeft = ref(865); // 14:25 in seconds
const spotsLeft = ref(3);
const isFabVisible = ref(true);
const scrollRef = ref(null);
const ctaRef = ref(null);

const successMessages = [
  { id: 1, user: "Mariana_92", text: "¡Perdí 8kg en un mes! Increíble 😍", type: "success" },
  { id: 2, user: "Carlos_Fit", text: "Mi metabolismo despertó al fin ⚡️", type: "info" },
  { id: 3, user: "Gaby_Luna", text: "Ropa que no me quedaba ya me queda 👗✨", type: "success" },
  { id: 4, user: "Juan_CDMX", text: "El Dr. Aaron es un genio, gracias Luckdate 🙌", type: "info" },
  { id: 5, user: "Sofi_Health", text: "Sin dietas locas, solo ciencia 🧬🧪", type: "success" },
  { id: 6, user: "Lupita_R", text: "¡Ya no tengo rebote! Felicidad pura ❤️", type: "success" },
  { id: 7, user: "Roberto_M", text: "Me siento con mucha energía hoy 🏃‍♂️🔥", type: "info" },
  { id: 8, user: "Elena_V", text: "Resultados reales, no puedo creerlo 😱💖", type: "success" },
  { id: 9, user: "Ximena_10", text: "Adiós azúcar, hola vida sana 🍎💪", type: "info" },
  { id: 10, user: "Tito_G", text: "Mi esposa y yo bajamos juntos 👫🏆", type: "success" },
];

let animationFrameId = null;
let scrollPos = 0;

const startBarrage = () => {
  if (!scrollRef.value) return;
  const scrollContainer = scrollRef.value;
  
  const scroll = () => {
    scrollPos += 0.5;
    if (scrollPos >= scrollContainer.scrollHeight / 2) {
      scrollPos = 0;
    }
    scrollContainer.scrollTop = scrollPos;
    animationFrameId = requestAnimationFrame(scroll);
  };
  
  animationFrameId = requestAnimationFrame(scroll);
};

const stopBarrage = () => {
  if (animationFrameId) {
    cancelAnimationFrame(animationFrameId);
  }
};

const handleScroll = () => {
  if (!ctaRef.value) return;
  const ctaPosition = ctaRef.value.getBoundingClientRect().top;
  const windowHeight = window.innerHeight;
  
  if (ctaPosition < windowHeight * 0.8) {
    isFabVisible.value = false;
  } else {
    isFabVisible.value = true;
  }
};

let timer = null;

onMounted(() => {
  timer = setInterval(() => {
    if (timeLeft.value > 0) {
      timeLeft.value--;
    }
  }, 1000);
  
  startBarrage();
  window.addEventListener('scroll', handleScroll);
});

onUnmounted(() => {
  if (timer) clearInterval(timer);
  stopBarrage();
  window.removeEventListener('scroll', handleScroll);
});

const formatTime = (seconds) => {
  const m = Math.floor(seconds / 60);
  const s = seconds % 60;
  return `${m < 10 ? '0' : ''}${m} : ${s < 10 ? '0' : ''}${s}`;
};

const handleWhatsAppClick = () => {
  const message = "Hola, me gustaría consultar sobre el plan de pérdida de peso metabólico de Luckdate. Tengo varios problemas metabólicos.";
  const phone = "17323435615"; // Updated official contact
  window.open(`https://wa.me/${phone}?text=${encodeURIComponent(message)}`, '_blank');
};

const transformationPhotos = import.meta.glob('./assets/user_photo/*.{png,jpg,jpeg}', { eager: true });
const sortedPhotos = Object.entries(transformationPhotos)
  .sort(([a], [b]) => {
    const numA = parseInt(a.match(/\d+/)[0]);
    const numB = parseInt(b.match(/\d+/)[0]);
    return numA - numB;
  })
  .map(([_, mod]) => mod.default);

const transformationCases = sortedPhotos.map((url, i) => ({
  id: i,
  url
}));

const isHeartPoint = (idx) => {
  const row = Math.floor(idx / 8);
  const col = idx % 8;
  const heartMask = [
    [0,1,1,0,0,1,1,0],
    [1,1,1,1,1,1,1,1],
    [1,1,1,1,1,1,1,1],
    [0,1,1,1,1,1,1,0],
    [0,0,1,1,1,1,0,0],
    [0,0,0,1,1,0,0,0]
  ];
  return heartMask[row]?.[col] === 1;
};

const localizedReviews = REVIEWS.map((review, index) => {
  const avatars = [person1, person2, person3];
  return {
    ...review,
    avatar: avatars[index] || review.avatar
  };
});
</script>

<template>
  <div class="min-h-screen flex flex-col items-center bg-white">
    <!-- Floating WhatsApp Button -->
    <div :class="`fixed bottom-6 right-6 z-[999] transition-all duration-500 ease-in-out ${isFabVisible ? 'opacity-100 translate-y-0 scale-100' : 'opacity-0 translate-y-10 scale-90 pointer-events-none'}`">
      <button 
        @click="handleWhatsAppClick"
        class="bg-[#25D366] text-white px-6 py-4 rounded-full shadow-2xl flex items-center space-x-3 hover:scale-110 active:scale-95 transition-all duration-300 group ring-4 ring-green-400/20"
      >
        <i class="fa-brands fa-whatsapp text-2xl"></i>
        <span class="font-black text-sm tracking-tight hidden md:block uppercase">Obtener mi plan personalizado</span>
        <span class="font-black text-sm tracking-tight md:hidden uppercase">Plan personalizado</span>
      </button>
    </div>

    <!-- Navigation -->
    <nav class="sticky top-0 z-50 w-full bg-white shadow-sm border-b border-slate-100">
      <div class="bg-red-600 text-white text-[12px] py-2 text-center font-bold tracking-widest flex items-center justify-center space-x-4">
        <span class="uppercase">Centro de Gestión Metabólica Certificado</span>
        <span class="w-px h-3 bg-white/30"></span>
        <span>Cupos gratuitos hoy: <span class="bg-white text-red-600 px-1.5 rounded mx-1">{{ spotsLeft }}</span></span>
      </div>
      <div class="page-container px-6 py-4 flex justify-between items-center">
        <div class="flex items-center">
          <img :src="logo" alt="Luckdate Clinic" class="h-10 w-auto" />
        </div>
        <div class="flex items-center">
          <button 
            @click="handleWhatsAppClick"
            class="bg-blue-600 text-white px-6 py-2 rounded-full hover:bg-blue-700 transition-all text-sm font-bold"
          >
            Consulta Online
          </button>
        </div>
      </div>
    </nav>

    <main class="w-full">
      <!-- Hero Section -->
      <header class="relative bg-[#f8fafc] overflow-hidden pt-12 pb-24 border-b border-slate-100">
        <div class="page-container px-6 grid lg:grid-cols-2 gap-16 items-center">
          <div class="text-center lg:text-left order-2 lg:order-1">
            <div class="flex items-center justify-center lg:justify-start space-x-4 mb-8">
              <img :src="nbeBadge" alt="Nobel Laureate" class="h-12 w-auto" />
              <div class="inline-flex items-center space-x-2 bg-yellow-50 text-yellow-700 px-4 py-2 rounded-full text-xs font-black border border-yellow-200">
                <i class="fa-solid fa-medal"></i>
                <span class="tracking-wider uppercase">PREMIO NOBEL DE QUÍMICA 2004</span>
              </div>
            </div>
            <h1 class="text-5xl lg:text-7xl font-black text-slate-900 leading-[1.1] mb-8">
              Reinicie su<br/>
              <span class="text-blue-600">Sistema Metabólico</span>
            </h1>
            <p class="text-xl text-slate-600 mb-10 max-w-2xl mx-auto lg:mx-0 font-medium leading-relaxed">
              Deje de preocuparse por las calorías y repare su metabolismo desde la fuente celular. Basado en la tecnología de "Ubiquitina-Proteasoma" de nivel Nobel para remodelar su cuerpo.
            </p>
            <div class="flex flex-wrap justify-center lg:justify-start gap-4 md:gap-12 border-t border-slate-200 pt-10">
              <div><p className="text-3xl font-black text-blue-900">10 años</p><p className="text-[11px] text-slate-400 font-bold uppercase tracking-widest mt-1">Investigación Clínica</p></div>
              <div><p className="text-3xl font-black text-blue-900">117</p><p className="text-[11px] text-slate-400 font-bold uppercase tracking-widest mt-1">Países Servidos</p></div>
              <div><p className="text-3xl font-black text-blue-900">1.34M+</p><p className="text-[11px] text-slate-400 font-bold uppercase tracking-widest mt-1">Casos de Éxito</p></div>
              <div><p className="text-3xl font-black text-blue-900">5.45M+</p><p className="text-[11px] text-slate-400 font-bold uppercase tracking-widest mt-1">Peso Perdido (kg)</p></div>
            </div>
          </div>
          <div class="relative order-1 lg:order-2 flex justify-center lg:justify-end">
            <div class="relative max-w-md xl:max-w-lg">
              <img :src="topImg" alt="Dr. Aaron Ciechanover" class="rounded-[3rem] shadow-2xl border-8 border-white object-cover aspect-[4/5] w-full" />
              <div class="absolute -bottom-8 -right-4 lg:-right-10 bg-white p-8 rounded-[2.5rem] shadow-2xl text-blue-900 max-w-sm border border-slate-100">
                <p class="text-lg font-black mb-1">Dr. Aaron Ciechanover</p>
                <p class="text-xs text-blue-600 font-bold uppercase tracking-widest mb-3">Premio Nobel</p>
                <p class="text-xs font-bold mb-4 text-slate-500">Experto Principal en Metabolismo de Luckdate Clinic</p>
                <p class="text-xs leading-relaxed text-slate-600 italic">"El metabolismo no es una simple suma y resta, sino una regulación compleja de la degradación de proteínas."</p>
              </div>
            </div>
          </div>
        </div>
      </header>

      <!-- Chat Interface -->
      <div class="w-full bg-white">
        <div class="page-container"><ChatInterface /></div>
      </div>

      <!-- Truth Section -->
      <section class="bg-slate-50 py-32 px-6">
        <div class="page-container">
          <div class="max-w-6xl mx-auto text-center">
            <h2 class="text-4xl lg:text-6xl font-black text-slate-900 mb-12 leading-tight">Déjeme decirle la verdad:<br/><span class="text-red-600 underline decoration-wavy decoration-red-200 underline-offset-8">¡No es culpa suya!</span></h2>
            <div class="bg-white p-12 rounded-[3rem] shadow-xl border border-slate-100 relative overflow-hidden text-left">
              <p class="text-2xl text-slate-700 leading-relaxed mb-10">Su situación actual es un caso típico de <strong class="text-blue-600">"Desequilibrio Metabólico"</strong>.</p>
              <div class="bg-green-50 p-8 rounded-2xl">
                <p class="text-xl text-slate-700 leading-relaxed font-medium">Este tipo de obesidad metabólica puede mejorarse sistemáticamente. Use tecnología científica de nivel Nobel para <span class="text-blue-600 font-bold">"reparar"</span> sus vías metabólicas.</p>
              </div>
            </div>
          </div>
        </div>
      </section>

      <!-- Science Section -->
      <section class="py-32 px-6 bg-white">
        <div class="page-container">
          <div class="text-center mb-24">
            <h2 class="text-4xl lg:text-5xl font-black mb-8 text-blue-900">Mecanismo de Reparación Metabólica de 6 Etapas</h2>
            <div class="flex flex-wrap justify-center gap-4 text-sm font-bold">
              <span v-for="step in ['Desintoxicación', 'Nutrición', 'Bloqueo', 'Activación', 'Regulación', 'Consolidación']" :key="step" class="bg-blue-50 text-blue-600 px-6 py-2 rounded-full border border-blue-100">{{ step }}</span>
            </div>
          </div>
          <div class="grid md:grid-cols-2 lg:grid-cols-3 gap-12 mb-24">
            <div v-for="(item, i) in [
              { title: 'Limpieza Celular', desc: 'Identifica y elimina con precisión las células envejecidas.', icon: 'fa-solid fa-wand-magic-sparkles', phase: '01' },
              { title: 'Suplementación', desc: 'Activan las enzimas responsables del transporte de energía.', icon: 'fa-solid fa-vial', phase: '02' },
              { title: 'Bloqueo Carbohidratos', desc: 'Evita picos de insulina que promueven el almacenamiento.', icon: 'fa-solid fa-shield-halved', phase: '03' },
              { title: 'Activación Quema', desc: 'Estimula la termogénesis celular para convertir grasa.', icon: 'fa-solid fa-fire-flame-simple', phase: '04' },
              { title: 'Regulación Hormonal', desc: 'Optimiza la secreción de leptina e insulina.', icon: 'fa-solid fa-scale-balanced', phase: '05' },
              { title: 'Consolidación', desc: 'Restablece el punto de ajuste metabólico.', icon: 'fa-solid fa-shield-heart', phase: '06' }
            ]" :key="i" class="group p-10 rounded-[3rem] bg-slate-50 border border-slate-100 hover:border-blue-500 hover:bg-white transition-all duration-500 shadow-md">
              <div class="w-16 h-16 bg-blue-600 text-white rounded-2xl flex items-center justify-center text-2xl mb-8"><i :class="item.icon"></i></div>
              <h3 class="text-2xl font-black mb-4 text-slate-900">{{ item.title }}</h3>
              <p class="text-slate-500 leading-relaxed font-medium">{{ item.desc }}</p>
            </div>
          </div>
        </div>
      </section>

      <!-- Reviews Section -->
      <section class="py-32 px-6 bg-[#f5f5f7] overflow-hidden relative">
        <div class="page-container relative z-10">
          <h2 class="text-4xl lg:text-5xl font-black text-center mb-20 text-slate-900">Más de 1,340,000 usuarios han recuperado su confianza</h2>
          <div class="grid lg:grid-cols-3 gap-8">
            <div v-for="review in localizedReviews" :key="review.id" class="bg-white p-8 rounded-3xl shadow-lg border border-slate-200 transition-all hover:shadow-xl">
              <div class="flex items-center space-x-4 mb-6">
                <img :src="review.avatar" :alt="review.name" class="w-14 h-14 rounded-full border-2 border-slate-100 object-cover shadow-sm" />
                <div>
                  <h4 class="font-bold text-slate-900 text-lg leading-tight">{{ review.name }} ({{ review.age }} años, {{ review.location }})</h4>
                  <div class="flex text-orange-400 mt-1">
                    <i v-for="i in 5" :key="i" class="fa-solid fa-star text-xs mr-0.5"></i>
                    <span class="text-xs text-slate-400 font-bold ml-2">Verificado ✅</span>
                  </div>
                </div>
              </div>
              <p class="text-slate-700 leading-relaxed font-medium">“{{ review.comment }}”</p>
              <div class="mt-6 pt-6 border-t border-slate-100 flex justify-between items-center">
                <span class="text-[10px] font-black text-slate-300 uppercase tracking-widest">Reseña del Cliente</span>
                <div class="flex space-x-2">
                   <i class="fa-solid fa-thumbs-up text-blue-500 text-xs"></i>
                   <span class="text-[10px] text-slate-400 font-bold">Útil</span>
                </div>
              </div>
            </div>
          </div>
        </div>
      </section>

      <!-- Community / Barrage Section -->
      <section class="py-24 bg-white relative overflow-hidden">
        <div class="page-container px-6 grid lg:grid-cols-12 gap-8 items-center">
          <div class="lg:col-span-3 h-[500px] relative order-2 lg:order-1 mt-12 lg:mt-0">
            <div class="absolute top-0 left-0 w-full h-20 bg-gradient-to-b from-white to-transparent z-10"></div>
            <div class="absolute bottom-0 left-0 w-full h-20 bg-gradient-to-t from-white to-transparent z-10"></div>
            <div class="bg-black/5 rounded-[2rem] p-4 h-full overflow-hidden relative backdrop-blur-sm border border-slate-100">
              <div class="flex items-center space-x-2 mb-4 px-2">
                <div class="bg-red-500 text-white text-[10px] font-black px-2 py-0.5 rounded animate-pulse uppercase">En Vivo</div>
                <div class="text-[10px] font-bold text-slate-400 uppercase tracking-widest">Chat de la Comunidad</div>
              </div>
              <div ref="scrollRef" class="h-full overflow-y-hidden space-y-3">
                <div v-for="(msg, i) in [...successMessages, ...successMessages, ...successMessages]" :key="`${msg.id}-${i}`" class="bg-white p-3 rounded-2xl shadow-sm border border-slate-50 flex flex-col">
                  <span class="text-[10px] font-black text-blue-600 mb-1">@{{ msg.user }}</span>
                  <p class="text-xs font-semibold text-slate-700 leading-snug">{{ msg.text }}</p>
                </div>
              </div>
            </div>
          </div>
          <div class="lg:col-span-9 relative order-1 lg:order-2">
            <div class="text-center lg:text-left mb-12">
              <h2 class="text-4xl font-black text-slate-900 mb-4">Hecho con Amor y Ciencia</h2>
              <p class="text-slate-500 font-medium text-lg leading-relaxed">Resultados reales que inspiran a nuestra comunidad en México y el mundo.</p>
            </div>
            <div class="relative flex justify-center lg:justify-start">
              <div class="grid grid-cols-4 sm:grid-cols-6 md:grid-cols-8 gap-2 max-w-5xl">
                <div v-for="(item, idx) in transformationCases" :key="item.id" :class="`relative aspect-square transition-all duration-700 ${isHeartPoint(idx) ? 'opacity-100 scale-100' : 'opacity-10 lg:opacity-5 scale-90'}`">
                  <img :src="item.url" class="w-full h-full object-cover rounded-lg shadow-sm border border-slate-100" />
                  <div v-if="isHeartPoint(idx)" class="absolute inset-0 bg-blue-600/10 rounded-lg"></div>
                </div>
              </div>
              <div class="absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 pointer-events-none opacity-20">
                <i class="fa-solid fa-heart text-[24rem] text-red-100 blur-2xl"></i>
              </div>
            </div>
          </div>
        </div>
      </section>

      <!-- CTA Section -->
      <section id="cta" ref="ctaRef" class="py-32 px-6 bg-white border-t border-slate-50 relative overflow-hidden">
        <div class="page-container max-w-6xl mx-auto text-center relative z-10">
          <div class="bg-red-50 text-red-600 px-6 py-2 rounded-full inline-flex items-center space-x-3 mb-8 border border-red-100">
             <span class="w-2 h-2 bg-red-600 rounded-full animate-ping"></span>
             <span class="font-black text-xs tracking-widest uppercase">ALERTA DE DISPONIBILIDAD</span>
          </div>
          
          <h2 class="text-4xl lg:text-6xl font-black text-slate-900 mb-6 leading-tight">
            Cada día se abren solo <span class="text-red-600 underline">20 plazas</span> de diagnóstico gratuito
          </h2>
          <p class="text-xl text-slate-500 mb-12 font-medium">
            El equipo de expertos del Dr. Aaron le proporcionará un plan de mejora metabólica 1 a 1 totalmente personalizado.
          </p>

          <div class="grid md:grid-cols-2 gap-8 mb-12">
             <div class="bg-[#f8fafc] p-8 rounded-3xl border-2 border-slate-100 flex flex-col justify-center">
                <p class="text-xs font-black text-slate-400 uppercase tracking-widest mb-3">La oferta expira en</p>
                <p class="text-5xl font-black text-red-600 tabular-nums tracking-tighter">[{{ formatTime(timeLeft) }}]</p>
             </div>
             <div class="bg-[#f0f9ff] p-8 rounded-3xl border-2 border-blue-100 flex flex-col justify-center items-center">
                <div class="flex items-center space-x-2 mb-2">
                   <span class="w-2.5 h-2.5 bg-green-500 rounded-full"></span>
                   <p class="text-lg font-black text-slate-900">Solo quedan <span class="text-blue-600 text-2xl">{{ spotsLeft }}</span> cupos</p>
                </div>
                <p class="text-[10px] text-blue-500 font-bold uppercase tracking-widest">Actualmente: 47 personas solicitando</p>
                <div class="w-full bg-blue-200 h-1.5 rounded-full mt-4 overflow-hidden">
                  <div class="bg-blue-600 h-full w-[85%] rounded-full"></div>
                </div>
             </div>
          </div>

          <div class="relative group">
             <div class="absolute -inset-1 bg-gradient-to-r from-green-400 to-green-600 rounded-[2.5rem] blur opacity-25 group-hover:opacity-50 transition duration-1000 group-hover:duration-200"></div>
             <button 
                @click="handleWhatsAppClick"
                class="relative w-full bg-[#25D366] hover:bg-[#128C7E] text-white py-10 rounded-[2.5rem] text-2xl lg:text-3xl font-black shadow-2xl flex flex-col items-center justify-center space-y-2 transition-all hover:scale-[1.02] active:scale-95"
             >
                <div class="flex items-center space-x-4">
                  <i class="fa-brands fa-whatsapp text-5xl"></i>
                  <span class="uppercase">Obtener mi plan personalizado</span>
                </div>
                <span class="text-sm font-bold opacity-80 uppercase tracking-widest">(Get My Personalized Metabolic Plan)</span>
             </button>
          </div>
          
          <p class="mt-8 text-xs font-bold text-slate-400 uppercase tracking-[0.2em] flex items-center justify-center space-x-4">
             <span>CONSULTA GRATUITA</span>
             <span class="w-1.5 h-1.5 bg-slate-300 rounded-full"></span>
             <span>SIN COMPRA OBLIGATORIA</span>
             <span class="w-1.5 h-1.5 bg-slate-300 rounded-full"></span>
             <span>100% PRIVACIDAD PROTEGIDA</span>
          </p>
        </div>
      </section>
    </main>

    <footer class="w-full bg-slate-900 text-slate-500 py-24 px-6">
      <div class="page-container border-t border-slate-800 pt-16 grid lg:grid-cols-2 gap-16 items-start">
        <div>
          <div class="flex items-center mb-8">
            <img :src="logo" alt="Luckdate" class="h-10 w-auto brightness-0 invert" />
          </div>
          <p class="max-w-md text-sm leading-relaxed mb-8">Centro líder en investigación metabólica.</p>
        </div>
        <div class="space-y-8">
          <p class="text-[11px] leading-relaxed italic text-slate-400">Descargo de responsabilidad: El contenido de esta página es solo para referencia informativa y no reemplaza el diagnóstico médico profesional. El Dr. Aaron Ciechanover proporciona orientación académica basada en su investigación Nobel. Los resultados varían según la constitución individual y el estilo de vida.</p>
          <p class="text-[10px] uppercase font-bold tracking-widest text-slate-700">© 2024 LUCKDATE CLINICAL METABOLIC CENTER. TODOS LOS DERECHOS RESERVADOS.</p>
        </div>
      </div>
    </footer>
  </div>
</template>
