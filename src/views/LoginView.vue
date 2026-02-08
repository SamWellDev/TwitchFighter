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
        <div class="relative z-10 container mx-auto px-4 h-screen flex flex-col items-center justify-center">

            <!-- Logo / Title -->
            <div class="text-center mb-12 relative group">
                <h1 class="text-6xl md:text-8xl font-black tracking-tighter uppercase italic transform -skew-x-6
                           bg-clip-text text-transparent bg-gradient-to-b from-blue-300 via-purple-400 to-purple-600
                           drop-shadow-[0_0_15px_rgba(168,85,247,0.5)]">
                    Twitch Fighter
                </h1>
                <!-- Glow effect behind title -->
                <div
                    class="absolute -inset-4 bg-purple-600/20 blur-3xl -z-10 rounded-full opacity-50 group-hover:opacity-75 transition-opacity duration-1000">
                </div>
            </div>

            <!-- Welcome Card -->
            <div
                class="w-full max-w-2xl bg-gray-900/60 backdrop-blur-xl border border-white/10 rounded-3xl p-8 md:p-12 text-center shadow-2xl relative overflow-hidden">
                <!-- Neon border top -->
                <div
                    class="absolute top-0 left-0 w-full h-1 bg-gradient-to-r from-transparent via-purple-500 to-transparent opacity-50">
                </div>

                <h2 class="text-4xl font-bold mb-6 text-white drop-shadow-md">Welcome!</h2>

                <p class="text-gray-300 text-lg mb-10 leading-relaxed">
                    Welcome to your epic landing page for Twitch Fighter! Connect your account to unleash the battle and
                    dominate the arena with your community.
                </p>

                <!-- Login Button -->
                <button @click="loginWithTwitch"
                    class="group relative inline-flex items-center justify-center px-8 py-4 font-bold text-white transition-all duration-200 bg-purple-600 font-lg rounded-xl focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-purple-600 hover:bg-purple-500 hover:scale-105 hover:shadow-[0_0_20px_rgba(168,85,247,0.5)]">
                    <span class="mr-3">
                        <svg class="w-6 h-6" viewBox="0 0 24 24" fill="currentColor">
                            <path
                                d="M11.571 4.714h1.715v5.143H11.57zm4.715 0H18v5.143h-1.714zM6 0L1.714 4.286v15.428h5.143V24l4.286-4.286h3.428L22.286 12V0zm14.571 11.143l-3.428 3.428h-3.429l-3 3v-3H6.857V1.714h13.714Z" />
                        </svg>
                    </span>
                    Login with Twitch
                    <div class="absolute inset-0 rounded-xl ring-2 ring-white/20 group-hover:ring-white/40"></div>
                </button>
            </div>

            <!-- Feature Cards (Footer) -->
            <div class="grid grid-cols-1 md:grid-cols-3 gap-6 w-full max-w-5xl mt-16">
                <!-- Card 1: Follows -->
                <div
                    class="group relative bg-gray-900/80 backdrop-blur-sm border border-cyan-500/30 rounded-xl p-6 hover:bg-gray-800/80 transition-all duration-300 hover:-translate-y-1">
                    <div
                        class="absolute inset-0 rounded-xl bg-gradient-to-br from-cyan-500/10 to-purple-500/10 opacity-0 group-hover:opacity-100 transition-opacity">
                    </div>
                    <div class="flex flex-col items-center justify-center relative z-10 text-center">
                        <div
                            class="mb-3 p-3 rounded-full bg-cyan-900/30 border border-cyan-500/50 text-cyan-400 shadow-[0_0_15px_rgba(6,182,212,0.3)]">
                            <svg xmlns="http://www.w3.org/2000/svg" width="32" height="32" viewBox="0 0 24 24"
                                fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round"
                                stroke-linejoin="round" class="lucide lucide-sword">
                                <line x1="14.5" x2="18.5" y1="17.5" y2="13.5" />
                                <path d="m11.5 14.5-3 3 5 5 3-3" />
                                <path d="m17.5 8.5 3 3" />
                                <path d="m8.5 17.5-3 3-5-5 3-3" />
                                <path d="M5.3 10.7 19.3 2.7c.7-.3 1.4.3 1 1L16 11.3" />
                            </svg>
                        </div>
                        <div class="text-cyan-400 font-bold tracking-wider text-sm md:text-base">FOLLOWS</div>
                        <div class="text-white text-xs mt-1">= Crit Damage</div>
                    </div>
                </div>

                <!-- Card 2: Subs -->
                <div
                    class="group relative bg-gray-900/80 backdrop-blur-sm border border-purple-500/30 rounded-xl p-6 hover:bg-gray-800/80 transition-all duration-300 hover:-translate-y-1">
                    <div
                        class="absolute inset-0 rounded-xl bg-gradient-to-br from-purple-500/10 to-pink-500/10 opacity-0 group-hover:opacity-100 transition-opacity">
                    </div>
                    <div class="flex flex-col items-center justify-center relative z-10 text-center">
                        <div
                            class="mb-3 p-3 rounded-full bg-purple-900/30 border border-purple-500/50 text-purple-400 shadow-[0_0_15px_rgba(168,85,247,0.3)]">
                            <svg xmlns="http://www.w3.org/2000/svg" width="32" height="32" viewBox="0 0 24 24"
                                fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round"
                                stroke-linejoin="round" class="lucide lucide-star">
                                <polygon
                                    points="12 2 15.09 8.26 22 9.27 17 14.14 18.18 21.02 12 17.77 5.82 21.02 7 14.14 2 9.27 8.91 8.26 12 2" />
                            </svg>
                        </div>
                        <div class="text-purple-400 font-bold tracking-wider text-sm md:text-base">SUBS</div>
                        <div class="text-white text-xs mt-1">= Attack Speed</div>
                    </div>
                </div>

                <!-- Card 3: Bits -->
                <div
                    class="group relative bg-gray-900/80 backdrop-blur-sm border border-emerald-500/30 rounded-xl p-6 hover:bg-gray-800/80 transition-all duration-300 hover:-translate-y-1">
                    <div
                        class="absolute inset-0 rounded-xl bg-gradient-to-br from-emerald-500/10 to-cyan-500/10 opacity-0 group-hover:opacity-100 transition-opacity">
                    </div>
                    <div class="flex flex-col items-center justify-center relative z-10 text-center">
                        <div
                            class="mb-3 p-3 rounded-full bg-emerald-900/30 border border-emerald-500/50 text-emerald-400 shadow-[0_0_15px_rgba(16,185,129,0.3)]">
                            <svg xmlns="http://www.w3.org/2000/svg" width="32" height="32" viewBox="0 0 24 24"
                                fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round"
                                stroke-linejoin="round" class="lucide lucide-coins">
                                <circle cx="8" cy="8" r="6" />
                                <path d="M18.09 10.37A6 6 0 1 1 10.34 18" />
                                <path d="M7 6h1v4" />
                                <path d="m16.71 13.88.7 .71-2.82 2.82" />
                            </svg>
                        </div>
                        <div class="text-emerald-400 font-bold tracking-wider text-sm md:text-base">BITS</div>
                        <div class="text-white text-xs mt-1">= Attack Power</div>
                    </div>
                </div>
            </div>

            <!-- Footer Text -->
            <div class="mt-8 text-gray-500 text-xs text-center">
                Twitch Fighter &copy; 2026 - Cyberpunk Dark Fantasy
            </div>
        </div>
    </div>
</template>

<script setup>
import { onMounted, ref } from 'vue'
import { useRouter } from 'vue-router'

const router = useRouter()
const showVideo = ref(true)

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
</script>

<style scoped>
/* Optional: Add custom animations or overrides here if Tailwind isn't enough */
</style>
