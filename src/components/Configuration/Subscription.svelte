<script lang="ts">
  import { ChevronLeft } from "lucide-svelte";

  export let activeSec;
</script>

<div
  id="help Center"
  class="w-full min-h-screen z-30 bg-black/[.23] grid place-items-center"
>
  <div
    class="rounded-[12px] bg-[#F5F6F7] flex flex-col items-start w-screen min-h-screen relative"
  >
    <button
      class="flex items-center justify-start text-black/[.7] pt-4"
      on:click={() => (activeSec = null)}
    >
      <ChevronLeft class="h-[30px] w-[30px]" />
      <p class="text-lg font-semibold">Subscription</p>
    </button>
    <div class="flex flex-col mt-[25px] px-4">
      <p class="text-[#333] font-bold text-2xl mb-[10px]">
        Everything you need at super{" "}
        <span class="text-[#015a62]">Affordable prices</span>
      </p>
      <p
        class="text-[rgb(18,17,39)]/[.56] text-xs leading-[23px] w-[310px] mb-[20px]"
      >
        Includes every feature we offer plus unlimited projects and unlimited
        users
      </p>
      <div
        class=" flex flex-col px-4 gap-1 overflow-y-scroll max-h-[calc(100vh-300px)] pb-32 hide-scrollbar"
      >
        <span
          class="flex flex-row pr-[11px] py-[18px] gap-4 items-center justify-start"
        >
          <img
            src="/images/checkSub.svg"
            alt="tick"
            class="h-[30px] w-[30px] aspect-square"
          />
          <p class="text-sm text-[#121127] font-semibold">
            Single platform for Network & Biometric devices.
          </p>
        </span>
        <span class="h-[1px] bg-[rgb(18,17,39)]/[.12]" />
        <span
          class="flex flex-row pr-[11px] py-[18px] gap-4 items-center justify-start"
        >
          <img
            src="/images/checkSub.svg"
            alt="tick"
            class="h-[30px] w-[30px] aspect-square"
          />
          <p class="text-sm text-[#121127] font-semibold">
            Robust alerting capabilities
          </p>
        </span>
        <span class="h-[1px] bg-[rgb(18,17,39)]/[.12]" />
        <span
          class="flex flex-row pr-[11px] py-[18px] gap-4 items-center justify-start"
        >
          <img
            src="/images/checkSub.svg"
            alt="tick"
            class="h-[30px] w-[30px] aspect-square"
          />
          <p class="text-sm text-[#121127] font-semibold">
            Incorporation of advanced analytics
          </p>
        </span>
        <span class="h-[1px] bg-[rgb(18,17,39)]/[.12]" />
        <span
          class="flex flex-row pr-[11px] py-[18px] gap-4 items-center justify-start"
        >
          <img
            src="/images/checkSub.svg"
            alt="tick"
            class="h-[30px] w-[30px] aspect-square"
          />
          <p class="text-sm text-[#121127] font-semibold">
            Emphasis on data privacy & security
          </p>
        </span>
        <span class="h-[1px] bg-[rgb(18,17,39)]/[.12]" />
        <span
          class="flex flex-row pr-[11px] py-[18px] gap-4 items-center justify-start"
        >
          <img
            src="/images/checkSub.svg"
            alt="tick"
            class="h-[30px] w-[30px] aspect-square"
          />
          <p class="text-sm text-[#121127] font-semibold">
            User-friendly interface for real-time monitoring
          </p>
        </span>
        <span class="h-[1px] bg-[rgb(18,17,39)]/[.12]" />
        <span
          class="flex flex-row pr-[11px] py-[18px] gap-4 items-center justify-start"
        >
          <img
            src="/images/checkSub.svg"
            alt="tick"
            class="h-[30px] w-[30px] aspect-square"
          />
          <p class="text-sm text-[#121127] font-semibold">
            AI driven features for enhanced security measures.
          </p>
        </span>
        <span class="h-[1px] bg-[rgb(18,17,39)]/[.12]" />
        <button class="text-[#015a62] ml-auto mt-[20px] font-medium text-sm">
          Learn more
        </button>
      </div>
    </div>
  </div>
</div>
