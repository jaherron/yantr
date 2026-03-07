<script setup>
import { ref } from 'vue'
import { ShieldAlert, Shield, CheckCircle, Loader, RefreshCw, Bell, Trash2 } from 'lucide-vue-next'
import { useApiUrl } from '../composables/useApiUrl'
import { useI18n } from 'vue-i18n'

const { apiUrl } = useApiUrl()
const { t } = useI18n()

const deploying = ref(false)
const deployError = ref('')
const deploySuccess = ref(false)

const features = [
  { icon: RefreshCw, label: t('watchtowerAlert.autoUpdates') },
  { icon: Bell, label: t('watchtowerAlert.notifications') },
  { icon: Trash2, label: t('watchtowerAlert.imageCleanup') },
]

async function deploy() {
  if (deploying.value) return
  deploying.value = true
  deployError.value = ''
  try {
    const res = await fetch(`${apiUrl.value}/api/deploy`, {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ appId: 'watchtower', environment: {} }),
    })
    const data = await res.json()
    if (!res.ok || !data.success) {
      deployError.value = data.error || data.message || 'Deployment failed'
    } else {
      deploySuccess.value = true
    }
  } catch (e) {
    deployError.value = e.message || 'Network error'
  } finally {
    deploying.value = false
  }
}
</script>

<template>
  <div class="relative group h-full flex flex-col bg-white dark:bg-[#0A0A0A] border border-gray-200 dark:border-zinc-800 rounded-xl overflow-hidden transition-all duration-300 hover:shadow-2xl hover:shadow-black/5 dark:hover:shadow-black/40 hover:border-gray-300 dark:hover:border-zinc-600 hover:-translate-y-0.5">
    <div class="absolute top-0 left-0 w-full h-0.5 bg-linear-to-r from-transparent via-amber-500 to-transparent opacity-0 group-hover:opacity-100 transition-opacity duration-500"></div>

    <transition
      enter-active-class="transition-all duration-500 ease-out"
      enter-from-class="opacity-0 scale-95"
      enter-to-class="opacity-100 scale-100"
    >
      <div v-if="deploySuccess" class="absolute inset-0 z-20 flex flex-col items-center justify-center bg-white/95 dark:bg-[#0A0A0A]/95 rounded-xl gap-3">
        <div class="w-12 h-12 rounded-full bg-green-50 dark:bg-green-500/10 border border-green-200 dark:border-green-500/20 flex items-center justify-center">
          <CheckCircle class="w-6 h-6 text-green-600 dark:text-green-500" />
        </div>
        <div class="text-center">
          <p class="text-sm font-semibold text-gray-900 dark:text-white tracking-tight">{{ t('watchtowerAlert.deployed') }}</p>
          <p class="text-[11px] text-gray-500 dark:text-zinc-400 mt-1 uppercase tracking-widest font-medium">{{ t('watchtowerAlert.containerStarting') }}</p>
        </div>
      </div>
    </transition>

    <div class="relative z-10 p-6 flex flex-col h-full gap-5">
      <div class="flex items-center justify-between gap-4">
        <div class="flex items-center gap-3 min-w-0">
          <div class="w-10 h-10 rounded-lg bg-gray-50 dark:bg-zinc-900 border border-gray-100 dark:border-zinc-800 flex items-center justify-center shrink-0 group-hover:scale-105 transition-all duration-500">
            <ShieldAlert class="w-5 h-5 text-amber-500" />
          </div>
          <div class="min-w-0">
            <h3 class="text-sm font-semibold text-gray-900 dark:text-white tracking-tight">Watchtower</h3>
            <div class="text-[11px] font-medium text-gray-500 dark:text-zinc-400 uppercase tracking-wider mt-1">{{ t('watchtowerAlert.security') }}</div>
          </div>
        </div>
        <div class="shrink-0 flex items-center gap-1.5">
          <div class="w-1.5 h-1.5 rounded-full bg-amber-500 animate-pulse"></div>
          <span class="text-[10px] font-bold uppercase tracking-wider text-amber-600 dark:text-amber-400">{{ t('watchtowerAlert.notInstalled') }}</span>
        </div>
      </div>

      <div class="grid grid-cols-1 gap-2 sm:grid-cols-3">
        <div
          v-for="feature in features"
          :key="feature.label"
          class="flex items-center gap-2 rounded-lg border border-gray-100 bg-gray-50 px-3 py-2.5 text-gray-600 dark:border-zinc-800/50 dark:bg-zinc-900/50 dark:text-zinc-300"
        >
          <component :is="feature.icon" class="h-3.5 w-3.5 shrink-0 text-amber-500" />
          <span class="text-[11px] font-medium leading-tight">{{ feature.label }}</span>
        </div>
      </div>

      <transition
        enter-active-class="transition-all duration-300 ease-out"
        enter-from-class="opacity-0 translate-y-1"
        enter-to-class="opacity-100 translate-y-0"
        leave-active-class="transition-all duration-150 ease-in"
        leave-from-class="opacity-100 translate-y-0"
        leave-to-class="opacity-0 translate-y-1"
      >
        <div v-if="deployError" class="flex items-center gap-2 p-2.5 rounded-lg bg-red-50 dark:bg-red-500/10 border border-red-100 dark:border-red-500/20">
          <ShieldAlert class="w-3.5 h-3.5 text-red-500 shrink-0" />
          <p class="text-[11px] text-red-600 dark:text-red-400 font-medium">{{ deployError }}</p>
        </div>
      </transition>

      <button
        @click="deploy"
        :disabled="deploying"
        class="mt-auto w-full flex items-center justify-center gap-2 px-4 py-3 rounded-lg text-[11px] font-bold uppercase tracking-wider transition-all duration-200 bg-gray-900 dark:bg-white text-white dark:text-gray-900 hover:-translate-y-0.5 hover:shadow-lg active:translate-y-0 cursor-pointer disabled:opacity-60 disabled:cursor-not-allowed disabled:hover:translate-y-0 disabled:hover:shadow-none"
      >
        <Loader v-if="deploying" class="w-3.5 h-3.5 animate-spin" />
        <Shield v-else class="w-3.5 h-3.5 transition-transform duration-200 group-hover:scale-110" />
        <span>{{ deploying ? t('watchtowerAlert.deploying') : t('watchtowerAlert.deployWatchtower') }}</span>
      </button>

    </div>
  </div>
</template>
