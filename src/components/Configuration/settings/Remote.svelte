<script lang="ts">
  import { onMount } from "svelte";

  let networkStatus = navigator.onLine;

  const checkNetworkStatus = () => {
    networkStatus = navigator.onLine;
  };

  onMount(() => {
    checkNetworkStatus();

    const onlineListener = () => {
      networkStatus = true;
    };

    const offlineListener = () => {
      networkStatus = false;
    };

    window.addEventListener("online", onlineListener);
    window.addEventListener("offline", offlineListener);

    return () => {
      window.removeEventListener("online", onlineListener);
      window.removeEventListener("offline", offlineListener);
    };
  });
</script>

<div class="h-[calc(100vh-75px)] w-full flex flex-col">
  <div
    class="flex-col mt-4 pt-8 shad mx-6 border-black/[.08] border-[1px] border-solid"
  >
    <span class="text-base font-medium dark:text-[#e0e0e0] text-[#212427] px-6">
      Internet Connectivity
    </span>
    <div
      class="h-[1px] dark:bg-[#292929] ml-2 mb-4 bg-[#e0e0e0] mt-8 w-[95%]"
    />
    <div class="flex items-center w-[60%] px-6 my-8 gap-4">
      {#if networkStatus}
        <span class="text-sm font-medium text-[#3B7C26]">online</span>
      {:else}
        <span class="text-sm font-medium text-[#EF4444]">offline</span>
      {/if}
      <span
        class="ml-2 mr-4 w-[320px] dark:text-[rgb(224,224,224)]/[.80] text-black/[.8] text-sm"
      >
        {networkStatus
          ? "  Internet connectivity found. This setup is remote compatible."
          : "   No internet connectivity found.This setup is remote incompatible."}
      </span>
      <span>
        {#if !networkStatus}
          <img
            src="/images/error.png"
            class="aspect-square h-[35px] w-[35px]"
            alt="network status"
          />
        {:else}
          <img
            alt="network status"
            src="/images/working.png"
            class="aspect-square h-[35px] w-[35px]"
          />
        {/if}
      </span>
    </div>
  </div>
  <div
    class="flex-col mt-4 pt-8 shad mx-6 border-black/[.08] border-[1px] border-solid"
  >
    <span
      class="text-base font-medium dark:text-[#e0e0e0] text-[#212427] px-6 py-8"
    >
      Node & Camera Connectivity
    </span>
    <div
      class="h-[1px] dark:bg-[#292929] ml-2 mb-4 bg-[#e0e0e0] mt-8 w-[95%]"
    />
    <div class="flex items-center w-[60%] px-6 my-8 gap-4">
      {#if networkStatus}
        <span class="text-sm font-medium text-[#3B7C26]">online</span>
      {:else}
        <span class="text-sm font-medium text-[#EF4444]">offline</span>
      {/if}
      <span
        class="ml-2 mr-4 w-[320px] dark:text-[rgb(224,224,224)]/[.80] text-black/[.8] text-sm"
      >
        {networkStatus
          ? "  Internet connectivity found. This setup is remote compatible."
          : "   No internet connectivity found.This setup is remote incompatible."}
      </span>
      <span>
        {#if !networkStatus}
          <img
            src="/images/error.png"
            class="aspect-square h-[35px] w-[35px]"
            alt="network status"
          />
        {:else}
          <img
            alt="network status"
            src="/images/working.png"
            class="aspect-square h-[35px] w-[35px]"
          />
        {/if}
      </span>
    </div>
  </div>

  <div
    class="flex-col mt-4 pt-8 shad mx-6 border-black/[.08] border-[1px] border-solid"
  >
    <span
      class="text-base font-medium dark:text-[#e0e0e0] text-[#212427] px-6 py-8"
    >
      Data being backed up
    </span>
    <div
      class="h-[1px] dark:bg-[#292929] ml-2 mb-4 bg-[#e0e0e0] mt-8 w-[95%]"
    />
    <div class="flex items-center w-[60%] px-6 my-8 gap-4">
      {#if networkStatus}
        <span class="text-sm font-medium text-[#3B7C26]">online</span>
      {:else}
        <span class="text-sm font-medium text-[#EF4444]">offline</span>
      {/if}
      <span
        class="ml-2 mr-4 w-[320px] dark:text-[rgb(224,224,224)]/[.80] text-black/[.8] text-sm"
      >
        {networkStatus
          ? "Events, Alerts, AI-detections all are not being recorded and backedup."
          : "Events, Alerts, AI-detections all are being recorded and backed up."}
      </span>
      <span>
        {#if !networkStatus}
          <img
            src="/images/error.png"
            class="aspect-square h-[35px] w-[35px]"
            alt="network status"
          />
        {:else}
          <img
            alt="network status"
            src="/images/working.png"
            class="aspect-square h-[35px] w-[35px]"
          />
        {/if}
      </span>
    </div>
  </div>
</div>

<style>
  .shad {
    box-shadow: 0px 4px 4px 0px #0000000f;
  }
</style>
