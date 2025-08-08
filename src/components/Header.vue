<template>
  <header>
    <nav class="bg-white border-gray-200 dark:bg-black px-4 lg:px-6 py-2.5 shadow-lg">
      <div class="flex flex-wrap justify-between items-center mx-auto max-w-screen-xl">
        <a href="https://flowbite.com" class="flex items-center">
          <img
            src="https://ci3.googleusercontent.com/meips/ADKq_NaAoQZBk3y-E_27tI8zlwu9wEvXsFT7zbPaEMnwrev3o_E2rA2ABVg_AgxgY1jgS_YF2ak609dmlQfDMY8mHMDfx8ZWhAKbIbcuWfWlWm-6=s0-d-e1-ft#https://dev.loosys.com.au/img/emails/logo-hirealoo-alt.png"
            class="mr-3 h-6 sm:h-9"
            alt="Flowbite Logo"
          />
        </a>
        <div class="flex items-center lg:order-2">
          <DropdownMenu>
            <DropdownMenuTrigger as-child>
              <Button variant="outline">
                <Icon
                  icon="radix-icons:moon"
                  class="h-[1.2rem] w-[1.2rem] rotate-0 scale-100 transition-all dark:-rotate-90 dark:scale-0"
                />
                <Icon
                  icon="radix-icons:sun"
                  class="absolute h-[1.2rem] w-[1.2rem] rotate-90 scale-0 transition-all dark:rotate-0 dark:scale-100"
                />
                <span class="sr-only">Toggle theme</span>
              </Button>
            </DropdownMenuTrigger>
            <DropdownMenuContent align="end">
              <DropdownMenuItem @click="mode = 'light'"> Light </DropdownMenuItem>
              <DropdownMenuItem @click="mode = 'dark'"> Dark </DropdownMenuItem>
              <DropdownMenuItem @click="mode = 'auto'"> System </DropdownMenuItem>
            </DropdownMenuContent>
          </DropdownMenu>
        </div>
      </div>
    </nav>
  </header>
</template>

<script setup lang="ts">
import { Icon } from '@iconify/vue'
import { useColorMode } from '@vueuse/core'
import { Button } from '@/components/ui/button'
import {
  DropdownMenu,
  DropdownMenuContent,
  DropdownMenuItem,
  DropdownMenuTrigger,
} from '@/components/ui/dropdown-menu'

// Pass { disableTransition: false } to enable transitions
const mode = useColorMode()
</script>
