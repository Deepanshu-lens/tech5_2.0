<script lang="ts">
  import { X } from "lucide-svelte";

  export let activeSec;
</script>

<div
  id="logout"
  class="w-full h-screen z-30 bg-black/[.23] grid place-items-center"
>
  <div
    class="rounded-[12px] bg-white flex flex-col items-start justify-center w-[85%] p-6 relative gap-6"
  >
    <button
      class="text-[#777777] absolute top-4 right-4"
      on:click={() => (activeSec = null)}
    >
      <X />
    </button>
    <span class="flex flex-row gap-4">
      <span
        class="bg-[#015a62]/[.08] rounded-lg h-[40px] w-[40px] grid place-items-center flex-shrink-0"
      >
        <img
          alt="log-out"
          src="/assets/vms/log-out2.svg"
          class="h-[24px] w-[24px] aspect-square object-contain"
        />
      </span>
      <span class="flex flex-col">
        <h4 class="text-[#042144] font-semibold mb-2">Logout</h4>
        <span class="text-[#828282] font-medium text-sm leading-[16.8px]">
          Are you sure you want to log out of this application?
        </span>
      </span>
    </span>
    <span class="flex flex-row items-center justify-center w-full gap-12">
      <button
        class="text-[#4F4F4F] font-medium text-base"
        on:click={() => (activeSec = "")}
      >
        Cancel
      </button>
      <span class="bg-[#dedede] w-[1px] h-[28px]" />
      <a href="/logout" class="text-[#015a62] font-semibold text-base">
        Log out
      </a>
    </span>
  </div>
</div>
