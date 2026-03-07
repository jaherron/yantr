<script setup>
import { useRouter } from "vue-router";
import { useI18n } from "vue-i18n";
import { ChevronRight, ClipboardList } from "lucide-vue-next";

const router = useRouter();
const { t } = useI18n();
</script>

<template>
  <div class="relative group h-full flex flex-col bg-white dark:bg-[#0A0A0A] border border-gray-200 dark:border-zinc-800 rounded-xl overflow-hidden transition-all duration-400 hover:shadow-2xl hover:shadow-black/5 dark:hover:shadow-black/40 hover:border-gray-300 dark:hover:border-zinc-600">
    <div class="absolute inset-x-0 top-0 h-0.5 bg-linear-to-r from-transparent via-emerald-500 to-transparent opacity-0 transition-opacity duration-500 group-hover:opacity-100"></div>

    <div class="relative z-10 flex flex-col h-full p-6">
      <div class="flex items-center gap-3 mb-6">
        <div class="w-10 h-10 rounded-lg bg-gray-50 dark:bg-zinc-900 border border-gray-100 dark:border-zinc-800 flex items-center justify-center shrink-0 group-hover:scale-105 transition-all duration-500">
          <ClipboardList class="w-5 h-5 text-gray-400 dark:text-zinc-500 group-hover:text-emerald-500 transition-colors" />
        </div>
        <div>
          <h3 class="text-sm font-semibold text-gray-900 dark:text-white tracking-tight group-hover:text-emerald-600 dark:group-hover:text-emerald-400 transition-colors">{{ t("home.toolsNavCard.logs") }}</h3>
          <div class="text-[11px] font-medium text-gray-500 dark:text-zinc-400 uppercase tracking-wider mt-1">{{ t("home.toolsNavCard.logsDesc") }}</div>
        </div>
      </div>

      <button
        type="button"
        @click="router.push('/logs')"
        class="mt-auto flex items-center justify-between p-3 rounded-lg bg-gray-50 dark:bg-zinc-900/50 border border-gray-100 dark:border-zinc-800/50 hover:border-gray-300 dark:hover:border-zinc-700 transition-all duration-300 focus:outline-none group/btn"
      >
        <div class="flex flex-col text-left min-w-0">
          <span class="text-xs font-semibold text-gray-900 dark:text-white">{{ t("home.toolsNavCard.logs") }}</span>
          <span class="text-[10px] text-gray-500 dark:text-zinc-400">{{ t("home.toolsNavCard.logsDesc") }}</span>
        </div>
        <ChevronRight class="w-4 h-4 text-gray-400 dark:text-zinc-600 group-hover/btn:text-gray-900 dark:group-hover/btn:text-white group-hover/btn:translate-x-1 transition-all" />
      </button>
    </div>
  </div>
</template>