<script setup lang="ts">
import { cva } from "class-variance-authority";
import { Eye, EyeOff, type LucideIcon } from "lucide-vue-next";

const props = defineProps<{
  id: string;
  title?: string;
  class?: string;
  icon?: LucideIcon;
  link?: string;
  href?: string;
  required?: boolean;
  isShowPass?: boolean;
  showPassword?: boolean;
  showPasswordClick?: () => void;
  variant?: "primary";
  borderRadius?: "none" | "medium" | "large" | "rounded";
  size?: "medium";
}>();

const inputVariants = cva("block w-full border border-gray-300 rounded-md", {
  variants: {
    variant: {
      primary: "focus:outline-none focus:ring-coral-500 focus:border-coral-500",
    },
    borderRadius: {
      none: "",
      medium: "rounded-md",
      large: "rounded-lg",
      rounded: "rounded-full",
    },
    size: { medium: "pl-10 pr-3 py-2 text-sm" },
  },
  defaultVariants: {
    variant: "primary",
    borderRadius: "medium",
    size: "medium",
  },
});

const className = cn(inputVariants({ variant: props.variant, borderRadius: props.borderRadius, class: props.class }), {
  "pr-4 pl-4": !props?.icon,
});
</script>

<template>
  <div>
    <div class="flex items-center justify-between">
      <label v-if="props.title" :for="props.id" class="flex items-center gap-1 text-sm font-medium text-gray-700">
        {{ props.title }} <span v-if="props.required" class="text-red-500 font-bold text-xl">*</span>
      </label>
      <a :href="props.href" class="text-sm text-coral-500 hover:text-coral-400"> {{ props.link }} </a>
    </div>

    <div class="mt-1 relative rounded-md shadow-sm">
      <div v-if="props?.icon" class="absolute inset-y-0 left-0 pl-3 flex items-center pointer-events-none">
        <props.icon class="h-5 w-5 text-gray-400" />
      </div>
      <input :id="props.id" v-bind="$attrs" :required="props.required" :class="className" />
      <button
        v-if="props.isShowPass"
        type="button"
        @click="showPasswordClick"
        class="absolute inset-y-0 right-0 pr-3 flex items-center"
      >
        <Eye v-if="!showPassword" class="h-5 w-5 text-gray-400" />
        <EyeOff v-else class="h-5 w-5 text-gray-400" />
      </button>
    </div>
  </div>
</template>
