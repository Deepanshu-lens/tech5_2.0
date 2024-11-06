<script lang="ts">
  // import { page } from "$app/stores";
  import Stream from "./settings/Stream.svelte";
  import Camera from "./settings/Camera.svelte";
  import Recording from "./settings/Recording.svelte";
  import Remote from "./settings/Remote.svelte";
  import System from "./settings/System.svelte";
  import UserS from "./settings/User.svelte";
  let search: string | null = null;
  import type { LoginEvent, User } from "@/types";
  import SettingsList from "./settings/SettingsList.svelte";
  // import { addUserLog } from "@/lib/addUserLog";
    // import { goto } from "@sveltejs/kit";
//   import { navigate } from "svelte-spa-router";
import { writable } from "svelte/store";
  import {
    CameraIcon,
    Cast,
    Disc2,
    FileSignatureIcon,
    Radio,
    Router,
    User2,
  } from "lucide-svelte";
  import License from "./settings/License.svelte";
  export let user: User;
  export let records: LoginEvent;
  export let logs: UserLog;
  export let data;
  import { selectedNode } from "@/stores";
  let currentSection: string = writable("Remote"); 
  $: {
    const searchParams = new URLSearchParams("");
    search = searchParams.get("section");
    if (search === null) {
      searchParams.set("section", "Remote");
      history.replaceState(null, "", `${location.pathname}?${searchParams}`);
      search = "Remote";
    }
  }


  const handleButtonClick = (text) => {
    //   goto(`/configuration/${sessionId}?section=${text}`);
    currentSection.set(text);
  };
</script>

<section
  class="right h-[calc(100vh-75px)] w-full flex-1 bg-background flex flex-row-reverse"
>
  <div
    class="flex flex-col gap-4 items-center justify-center pl-2 pr-4 h-full my-auto"
  >
    <span class="group flex flex-col gap-0.5 items-center justify-center">
      <button
      on:click={() => {
        handleButtonClick("Remote");
      }}
        class={$currentSection !== "Remote"
          ? `text-[#727272]/90 h-[40px] w-[40px] rounded-full   border border-solid border-[#727272]/90 dark:border-white/[.23] bg-white dark:bg-black dark:text-white group-hover:text-white group-hover:bg-[linear-gradient(90deg,#113046_0%,#2B6365_100%)] dark:group-hover:bg-[#258d9d] group-hover:border-none grid place-items-center `
          : `relative border-none rounded-full  h-[40px] w-[40px] text-white bg-[linear-gradient(90deg,#113046_0%,#2B6365_100%)] grid place-items-center dark:bg-[#258d9d]`}
        ><Router class="h-[22px] w-[22px]" /></button
      >
      <p
        class={`text-xs ${search !== "Remote" ? "group-hover:text-[#015a62] text-[#727272]/90 dark:text-white dark:group-hover:text-[#258d9d]" : "dark:text-[#258d9d]  text-[#015a62]"}`}
      >
        Remote
      </p>
    </span>
    <span class="group flex flex-col gap-0.5 items-center justify-center">
      <button
         on:click={() => {
        handleButtonClick("Stream");
      }}
        class={$currentSection !== "Stream"
          ? `text-[#727272]/90 h-[40px] w-[40px] rounded-full   border border-solid border-[#727272]/90 dark:border-white/[.23] bg-white dark:bg-black dark:text-white group-hover:text-white group-hover:bg-[linear-gradient(90deg,#113046_0%,#2B6365_100%)] dark:group-hover:bg-[#258d9d] group-hover:border-none grid place-items-center `
          : `relative border-none rounded-full  h-[40px] w-[40px] text-white bg-[linear-gradient(90deg,#113046_0%,#2B6365_100%)] grid place-items-center dark:bg-[#258d9d]`}
        ><Radio class="h-[22px] w-[22px]" />
      </button>
      <p
        class={`text-xs ${search !== "Stream" ? "group-hover:text-[#015a62] text-[#727272]/90 dark:text-white dark:group-hover:text-[#258d9d]" : "dark:text-[#258d9d]  text-[#015a62]"}`}
      >
        Stream
      </p>
    </span>
    <span class="group flex flex-col gap-0.5 items-center justify-center">
      <button
        on:click={() => {
          handleButtonClick("Recording");
        }}
        class={$currentSection !== "Recording"
          ? `text-[#727272]/90 h-[40px] w-[40px] rounded-full   border border-solid border-[#727272]/90 dark:border-white/[.23] bg-white dark:bg-black dark:text-white group-hover:text-white group-hover:bg-[linear-gradient(90deg,#113046_0%,#2B6365_100%)] dark:group-hover:bg-[#258d9d] group-hover:border-none grid place-items-center `
          : `relative border-none rounded-full  h-[40px] w-[40px] text-white bg-[linear-gradient(90deg,#113046_0%,#2B6365_100%)] grid place-items-center dark:bg-[#258d9d]`}
        ><Disc2 class="h-[22px] w-[22px]" />
      </button>
      <p
        class={`text-xs ${search !== "Recording" ? "group-hover:text-[#015a62] text-[#727272]/90 dark:text-white dark:group-hover:text-[#258d9d]" : "dark:text-[#258d9d]  text-[#015a62]"}`}
      >
        Recording
      </p>
    </span>
    <span class="group flex flex-col gap-0.5 items-center justify-center">
      <button
        on:click={() => {
          handleButtonClick("System");
        }}
        class={$currentSection !== "System"
          ? `text-[#727272]/90 h-[40px] w-[40px] rounded-full   border border-solid border-[#727272]/90 dark:border-white/[.23] bg-white dark:bg-black dark:text-white group-hover:text-white group-hover:bg-[linear-gradient(90deg,#113046_0%,#2B6365_100%)] dark:group-hover:bg-[#258d9d] group-hover:border-none grid place-items-center `
          : `relative border-none rounded-full  h-[40px] w-[40px] text-white bg-[linear-gradient(90deg,#113046_0%,#2B6365_100%)] grid place-items-center dark:bg-[#258d9d]`}
        ><Cast class="h-[22px] w-[22px]" />
      </button>
      <p
        class={`text-xs ${search !== "System" ? "group-hover:text-[#015a62] text-[#727272]/90 dark:text-white dark:group-hover:text-[#258d9d]" : "dark:text-[#258d9d]  text-[#015a62]"}`}
      >
        System
      </p>
    </span>
    <span class="group flex flex-col gap-0.5 items-center justify-center">
      <button
        on:click={() => {
          handleButtonClick("Camera");
        }}
        class={$currentSection !== "Camera"
          ? `text-[#727272]/90 h-[40px] w-[40px] rounded-full   border border-solid border-[#727272]/90 dark:border-white/[.23] bg-white dark:bg-black dark:text-white group-hover:text-white group-hover:bg-[linear-gradient(90deg,#113046_0%,#2B6365_100%)] dark:group-hover:bg-[#258d9d] group-hover:border-none grid place-items-center `
          : `relative border-none rounded-full  h-[40px] w-[40px] text-white bg-[linear-gradient(90deg,#113046_0%,#2B6365_100%)] grid place-items-center dark:bg-[#258d9d]`}
        ><CameraIcon class="h-[22px] w-[22px]" />
      </button>
      <p
        class={`text-xs ${search !== "Camera" ? "group-hover:text-[#015a62] text-[#727272]/90 dark:text-white dark:group-hover:text-[#258d9d]" : "dark:text-[#258d9d]  text-[#015a62]"}`}
      >
        Camera
      </p>
    </span>
    <span class="group flex flex-col gap-0.5 items-center justify-center">
      <button
        on:click={() => {
          handleButtonClick("License");
        }}
        class={$currentSection !== "License"
          ? `text-[#727272]/90 h-[40px] w-[40px] rounded-full   border border-solid border-[#727272]/90 dark:border-white/[.23] bg-white dark:bg-black dark:text-white group-hover:text-white group-hover:bg-[linear-gradient(90deg,#113046_0%,#2B6365_100%)] dark:group-hover:bg-[#258d9d] group-hover:border-none grid place-items-center `
          : `relative border-none rounded-full  h-[40px] w-[40px] text-white bg-[linear-gradient(90deg,#113046_0%,#2B6365_100%)] grid place-items-center dark:bg-[#258d9d]`}
        ><FileSignatureIcon class="h-[22px] w-[22px]" />
      </button>
      <p
        class={`text-xs ${search !== "License" ? "group-hover:text-[#015a62] text-[#727272]/90 dark:text-white dark:group-hover:text-[#258d9d]" : "dark:text-[#258d9d]  text-[#015a62]"}`}
      >
        License
      </p>
    </span>
    <span class="group flex flex-col gap-0.5 items-center justify-center">
      <button
        on:click={() => {
          handleButtonClick("User");
        }}
        class={$currentSection !== "User"
          ? `text-[#727272]/90 h-[40px] w-[40px] rounded-full   border border-solid border-[#727272]/90 dark:border-white/[.23] bg-white dark:bg-black dark:text-white group-hover:text-white group-hover:bg-[linear-gradient(90deg,#113046_0%,#2B6365_100%)] dark:group-hover:bg-[#258d9d] group-hover:border-none grid place-items-center `
          : `relative border-none rounded-full  h-[40px] w-[40px] text-white bg-[linear-gradient(90deg,#113046_0%,#2B6365_100%)] grid place-items-center dark:bg-[#258d9d]`}
        ><User2 class="h-[22px] w-[22px]" />
      </button>
      <p
        class={`text-xs ${search !== "User" ? "group-hover:text-[#015a62] text-[#727272]/90 dark:text-white dark:group-hover:text-[#258d9d]" : "dark:text-[#258d9d]  text-[#015a62]"}`}
      >
        User
      </p>
    </span>
  </div>
  <div class="w-full h-full">
    {#if $currentSection === "Remote"}
      <Remote />
    {:else if $currentSection === "Stream"}
      <Stream />
    {:else if $currentSection === "Recording"}
      <Recording />
    {:else if $currentSection === "System"}
      <System />
    {:else if $currentSection === "Camera"}
      <Camera {user} />
      <!-- {data} -->
    {:else if $currentSection === "License"}
      <License session={data?.session} />
    {:else}
      <UserS {user} {records} {logs} {data} />
    {/if}
  </div>

  <!-- <div
      class="h-[calc(100vh-75px)] w-1/4 max-w-72 dark:border-[#292929] border-l-[1px]"
    >
      <SettingsList {sessionId} />
    </div> -->
</section>
