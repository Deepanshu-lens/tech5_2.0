<script lang="ts">
    import { Disc2, Router, Camera, Radio, Cast, User } from "lucide-svelte";
    // import { page } from "$app/stores";
    // import { goto } from "$app/navigation";
    // import { addUserLog } from "@/lib/addUserLog";
  
    export let sessionId: string;
    let selectedButton = "Remote";
  
    const searchParams = new URLSearchParams("");
    const search = searchParams.get("section");
  
    const buttonData = [
      { text: "Remote" },
      { text: "Stream" },
      { text: "Recording" },
      { text: "System" },
      { text: "Camera" },
      { text: "User" },
    ];
  
    const handleButtonClick = (text) => {
      selectedButton = text;
    //   goto(`/configuration/${sessionId}?section=${text}`);
    };
  
    $: selectedButton = search ? search : "Remote";
  </script>
  
  <div class="left flex flex-col parent bg-background w-full h-full">
    <div
      class="relative py-10 px-6 flex flex-col gap-6 items-start justify-center"
    >
      {#each buttonData as button, index}
        <button
          on:click={() => {
            handleButtonClick(button.text);
            // addUserLog(`user clicked on setting list button ${button.text}`);
          }}
          class="w-full rounded-xl text-[#727272]/90 dark:text-[#e0e0e0] flex items-center justify-start gap-4 font-medium text-base px-4 py-3 z-20 {selectedButton !==
          button.text
            ? 'hover:bg-[rgba(1,90,98,.12)] dark:hover:bg-[#333]'
            : 'bg-[rgba(1,90,98,.12)] dark:bg-[#333]'}"
        >
          {#if button.text === "Remote"}
            <Router />
          {:else if button.text === "Stream"}
            <Radio />
          {:else if button.text === "Recording"}
            <Disc2 />
          {:else if button.text === "System"}
            <Cast />
          {:else if button.text === "Camera"}
            <Camera />
          {:else}
            <User />
          {/if}
          {button.text}
        </button>
      {/each}
    </div>
  </div>
  