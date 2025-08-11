<script lang="ts">
  import { Switch as SwitchPrimitive } from "bits-ui";
  import { cn, type WithoutChildrenOrChild } from "$lib/utils.js";

  let { ref = $bindable(null), class: className, checked = $bindable(false), ...restProps }: WithoutChildrenOrChild<SwitchPrimitive.RootProps> = $props();
</script>

<SwitchPrimitive.Root
  bind:ref
  bind:checked
  data-slot="switch"
  class={cn(
    "data-[state=checked]:bg-emerald-500 data-[state=unchecked]:bg-gray-200 focus-visible:ring-emerald-300 peer inline-flex h-6 w-11 shrink-0 items-center rounded-full border border-transparent outline-none transition-all focus-visible:ring-4 disabled:cursor-not-allowed disabled:opacity-50",
    className
  )}
  {...restProps}
>
  <SwitchPrimitive.Thumb data-slot="switch-thumb" class={cn("bg-white pointer-events-none block h-5 w-5 rounded-full shadow-sm ring-0 transition-transform data-[state=checked]:translate-x-5 data-[state=unchecked]:translate-x-0")} />
</SwitchPrimitive.Root>
