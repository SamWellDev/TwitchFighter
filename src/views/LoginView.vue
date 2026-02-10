<template>
    <div
        class="min-h-screen bg-gray-900 text-white font-sans selection:bg-purple-500 selection:text-white overflow-hidden relative">

        <!-- Video Background -->
        <video v-if="showVideo" autoplay loop muted playsinline
            class="absolute inset-0 w-full h-full object-cover opacity-100 z-0">
            <source src="/vids/Cinematic_Game_Start_Screen_Loop.webm" type="video/webm">
            Your browser does not support the video tag.
        </video>

        <!-- Static Background (Image) -->
        <div v-else class="absolute inset-0 bg-[url('/imgs/BG01.png')] bg-cover bg-center z-0"></div>

        <!-- Dark Overlay - Always present for text readability -->
        <div class="absolute inset-0 bg-gray-900/40 z-0 pointer-events-none"></div>

        <!-- Music Player (Top Left) -->
        <div
            class="absolute top-6 left-6 z-50 flex items-center gap-3 bg-gray-900/50 backdrop-blur-md border border-white/10 rounded-full px-4 py-2 hover:bg-white/10 transition-all duration-300">
            <button @click="toggleMusic"
                class="text-white opacity-70 hover:opacity-100 transition-opacity flex-shrink-0">
                <!-- Play icon -->
                <svg v-if="!isPlaying" xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24"
                    fill="currentColor">
                    <path d="M8 5v14l11-7z" />
                </svg>
                <!-- Pause icon -->
                <svg v-else xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24"
                    fill="currentColor">
                    <path d="M6 19h4V5H6v14zm8-14v14h4V5h-4z" />
                </svg>
            </button>
            <div class="overflow-hidden w-40 md:w-56">
                <div class="marquee-track">
                    <span class="text-white/70 text-sm whitespace-nowrap">🎵 Look at the abyss that is my eyes</span>
                    <span class="text-white/70 text-sm whitespace-nowrap">🎵 Look at the abyss that is my eyes</span>
                </div>
            </div>
        </div>
        <audio ref="audioPlayer" loop preload="auto" src="/song/Look at the abyss that is my eyes.mp3"></audio>

        <!-- Background Toggle Button (Top Right) -->
        <button @click="toggleBackground"
            class="absolute top-6 right-6 z-50 bg-gray-900/50 backdrop-blur-md border border-white/10 p-3 rounded-full hover:bg-white/10 transition-all duration-300 group">
            <!-- Icon changes based on state -->
            <svg v-if="showVideo" xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24"
                fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"
                class="text-white opacity-70 group-hover:opacity-100">
                <rect width="18" height="18" x="3" y="3" rx="2" ry="2" />
                <circle cx="9" cy="9" r="2" />
                <path d="m21 15-3.086-3.086a2 2 0 0 0-2.828 0L6 21" />
            </svg>
            <svg v-else xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none"
                stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"
                class="text-white opacity-70 group-hover:opacity-100">
                <path d="m22 8-6 4 6 4V8Z" />
                <rect width="14" height="12" x="2" y="6" rx="2" ry="2" />
            </svg>
            <span class="sr-only">Toggle Background</span>
        </button>

        <!-- Main Content Container -->
        <div class="relative z-10 container mx-auto px-4 h-screen flex flex-col items-center justify-between py-12">

            <!-- Logo / Title -->
            <div class="text-center top-[-50px] relative group">
                <img src="/imgs/logo.png" alt="Twitch Fighter"
                    class="max-h-40 md:max-h-56 object-contain mx-auto drop-shadow-[0_0_15px_rgba(168,85,247,0.5)]" />
                <!-- Glow effect behind logo -->
                <div
                    class="absolute -inset-4 bg-purple-600/20 blur-3xl -z-10 rounded-full opacity-50 group-hover:opacity-75 transition-opacity duration-1000">
                </div>
            </div>

            <!-- Bottom Content: Login + Cards + Footer -->
            <div class="flex flex-col items-center w-full mt-auto" style="margin-left: -10px;">
                <!-- Login Button -->
                <button @click="loginWithTwitch"
                    class="group relative inline-flex items-center justify-center px-10 py-5 font-bold text-lg text-white transition-all duration-200 bg-purple-600 rounded-xl focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-purple-600 hover:bg-purple-500 hover:scale-105 hover:shadow-[0_0_30px_rgba(168,85,247,0.6)]">
                    <span class="mr-3">
                        <svg class="w-7 h-7" viewBox="0 0 24 24" fill="currentColor">
                            <path
                                d="M11.571 4.714h1.715v5.143H11.57zm4.715 0H18v5.143h-1.714zM6 0L1.714 4.286v15.428h5.143V24l4.286-4.286h3.428L22.286 12V0zm14.571 11.143l-3.428 3.428h-3.429l-3 3v-3H6.857V1.714h13.714Z" />
                        </svg>
                    </span>
                    Login with Twitch
                    <div class="absolute inset-0 rounded-xl ring-2 ring-white/20 group-hover:ring-white/40"></div>
                </button>

                <!-- Demo Link -->
                <button @click="enterDemo"
                    class="mt-3 text-gray-500 hover:text-gray-300 text-sm transition-colors underline underline-offset-4 decoration-gray-600 hover:decoration-gray-400">
                    DEMO
                </button>

                <!-- Feature Cards -->
                <div class="grid grid-cols-1 md:grid-cols-3 gap-6 w-full max-w-5xl mt-8">
                    <!-- Card 1: Follows -->
                    <div
                        class="group relative bg-gray-900/80 backdrop-blur-sm border border-cyan-500/30 rounded-xl p-6 hover:bg-gray-800/80 transition-all duration-300 hover:-translate-y-1">
                        <div
                            class="absolute inset-0 rounded-xl bg-gradient-to-br from-cyan-500/10 to-purple-500/10 opacity-0 group-hover:opacity-100 transition-opacity">
                        </div>
                        <div class="flex items-center justify-center relative z-10">
                            <img src="/imgs/follow.png" alt="Follows" class="max-h-[200px] object-contain" />
                        </div>
                    </div>

                    <!-- Card 2: Subs -->
                    <div
                        class="group relative bg-gray-900/80 backdrop-blur-sm border border-purple-500/30 rounded-xl p-6 hover:bg-gray-800/80 transition-all duration-300 hover:-translate-y-1">
                        <div
                            class="absolute inset-0 rounded-xl bg-gradient-to-br from-purple-500/10 to-pink-500/10 opacity-0 group-hover:opacity-100 transition-opacity">
                        </div>
                        <div class="flex items-center justify-center relative z-10">
                            <img src="/imgs/sub.png" alt="Subs" class="max-h-[200px] object-contain" />
                        </div>
                    </div>

                    <!-- Card 3: Bits -->
                    <div
                        class="group relative bg-gray-900/80 backdrop-blur-sm border border-emerald-500/30 rounded-xl p-6 hover:bg-gray-800/80 transition-all duration-300 hover:-translate-y-1">
                        <div
                            class="absolute inset-0 rounded-xl bg-gradient-to-br from-emerald-500/10 to-cyan-500/10 opacity-0 group-hover:opacity-100 transition-opacity">
                        </div>
                        <div class="flex items-center justify-center relative z-10">
                            <img src="/imgs/bits.png" alt="Bits" class="max-h-[200px] object-contain" />
                        </div>
                    </div>
                </div>

                <!-- Footer -->
                <div class="mt-4 mb-2 w-full">
                    <div class="flex flex-col md:flex-row items-center justify-between gap-2">
                        <p class="text-gray-400 text-sm">
                            © 2026 Twitch Fighter • Developed by
                            <a href="https://portfoliosamwell.netlify.app/" target="_blank"
                                class="text-purple-400 hover:text-purple-300 transition-colors">
                                Samuel Araújo
                            </a>
                        </p>
                        <a href="https://github.com/SamWellDev/DPSVisualizer" target="_blank"
                            class="text-gray-400 hover:text-white text-sm flex items-center gap-1 transition-colors">
                            <svg class="w-4 h-4" viewBox="0 0 24 24" fill="currentColor">
                                <path
                                    d="M12 0c-6.626 0-12 5.373-12 12 0 5.302 3.438 9.8 8.207 11.387.599.111.793-.261.793-.577v-2.234c-3.338.726-4.033-1.416-4.033-1.416-.546-1.387-1.333-1.756-1.333-1.756-1.089-.745.083-.729.083-.729 1.205.084 1.839 1.237 1.839 1.237 1.07 1.834 2.807 1.304 3.492.997.107-.775.418-1.305.762-1.604-2.665-.305-5.467-1.334-5.467-5.931 0-1.311.469-2.381 1.236-3.221-.124-.303-.535-1.524.117-3.176 0 0 1.008-.322 3.301 1.23.957-.266 1.983-.399 3.003-.404 1.02.005 2.047.138 3.006.404 2.291-1.552 3.297-1.23 3.297-1.23.653 1.653.242 2.874.118 3.176.77.84 1.235 1.911 1.235 3.221 0 4.609-2.807 5.624-5.479 5.921.43.372.823 1.102.823 2.222v3.293c0 .319.192.694.801.576 4.765-1.589 8.199-6.086 8.199-11.386 0-6.627-5.373-12-12-12z" />
                            </svg>
                            GitHub
                        </a>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script setup>
import { onMounted, onUnmounted, ref } from 'vue'
import { useRouter } from 'vue-router'

const router = useRouter()
const showVideo = ref(true)
const isPlaying = ref(false)
const audioPlayer = ref(null)

// Backend auth URL
const BACKEND_AUTH_URL = 'http://localhost:5041/api/auth/login'

// Check if already logged in
onMounted(() => {
    const twitchId = localStorage.getItem('twitch_id')
    if (twitchId) {
        router.push('/dashboard')
    }
})

const loginWithTwitch = () => {
    // Redirect to backend which handles OAuth
    window.location.href = BACKEND_AUTH_URL
}

const toggleBackground = () => {
    showVideo.value = !showVideo.value
}

const enterDemo = () => {
    router.push('/dashboard?demo=true')
}

const toggleMusic = () => {
    if (!audioPlayer.value) return
    if (isPlaying.value) {
        audioPlayer.value.pause()
    } else {
        audioPlayer.value.play()
    }
    isPlaying.value = !isPlaying.value
}

onUnmounted(() => {
    if (audioPlayer.value) {
        audioPlayer.value.pause()
    }
})
</script>

<style scoped>
.marquee-track {
    display: flex;
    gap: 3rem;
    animation: marquee 12s linear infinite;
}

.marquee-track span {
    flex-shrink: 0;
}

@keyframes marquee {
    0% {
        transform: translateX(0);
    }

    100% {
        transform: translateX(-50%);
    }
}
</style>
