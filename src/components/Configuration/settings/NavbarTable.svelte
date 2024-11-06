<script>
  import { Button } from "@/components/ui/button";
  import Checkbox from "@/components/ui/checkbox/checkbox.svelte";
  import * as Table from "@/components/ui/table/index";
  export let allUsers;
  import { onMount } from "svelte";
  import { toast } from "svelte-sonner";
  import { writable, get } from "svelte/store";
  let reportsFeatures = [];
  let userFeatures = writable([]);

  onMount(async () => {
    const reports = await fetch("/api/features/navbar");
    if (!reports.ok) {
      throw new Error(
        `failed to fetch playback features, HTTP error! status: ${reports.status}`,
      );
    }
    const p = await reports.json();
    console.log(p);
    reportsFeatures = p?.features?.items;
    initializeUserFeatures();
  });
  function initializeUserFeatures() {
    const features = allUsers?.map((user) => ({
      id: user.id,
      features: user.features ? [...user.features] : [],
    }));
    userFeatures.set(features);
  }

  function handleFeatureChange(userId, featureId, isChecked) {
    console.log(userId, featureId, isChecked);
    let currentFeatures = get(userFeatures);
    const userIndex = currentFeatures.findIndex((u) => u.id === userId);
    if (userIndex !== -1) {
      const user = currentFeatures[userIndex];
      if (isChecked) {
        if (!user.features.includes(featureId)) {
          user.features.push(featureId);
          console.log("first");
        }
      } else {
        user.features = user.features.filter((f) => f !== featureId);
        console.log("second");
      }
      userFeatures.update((features) => {
        features[userIndex] = user;
        return features;
      });
    }
  }

  function handleFeaturesUpdate() {
    fetch("/api/features/addFeature", {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify($userFeatures),
    })
      .then((response) => response.json())
      .then((data) => {
        console.log("Success:", data);
        toast("Permissions Updated for Navbar!");
      })
      .catch((error) => {
        console.error("Error:", error);
        toast.error("Error in updating permissions!");
      });
  }
</script>

<Table.Root
  class="mx-auto h-full w-full flex flex-col max-w-[calc(100vw-8.3rem)] max-h-[calc(100vh-310px)] hide-scrollbar overflow-x-auto"
>
  <Table.Header
    class="border-2 border-[#e4e4e4] border-solid rounded-lg bg-[#f9f9f9]"
  >
    <Table.Row class="bg-transparent flex items-center gap-4 p-3">
      <Table.Head class="text-[#727272] h-full w-[50px]"
        ><Checkbox
          class="data-[state=checked]:bg-orange-500 data-[state=checked]:border-orange-500"
        />
      </Table.Head>
      <Table.Head class="text-[#727272] h-full w-[300px]">User Name</Table.Head>
      {#if reportsFeatures}
        {#each reportsFeatures as feature}
          <Table.Head class="text-[#727272] h-full w-[200px]"
            >{feature.feature}</Table.Head
          >
        {/each}
      {/if}
    </Table.Row>
  </Table.Header>
  <Table.Body class="pb-10">
    {#each allUsers as user}
      <Table.Row
        class="bg-transparent cursor-pointer flex items-center gap-4 mt-4 px-3 rounded-lg  border-2 border-solid border-[#e4e4e4]"
      >
        <Table.Cell class="text-black h-full w-[50px]">
          <Checkbox
            class="data-[state=checked]:bg-orange-500 data-[state=checked]:border-orange-500"
          />
        </Table.Cell>
        <Table.Cell class="text-black h-full">
          <span class="flex flex-col font-semibold text-primary">
            <span>
              Name: {user?.name.length > 0 ? user.name : "-"}
            </span>
            <span class="text-xs">
              Id: {user.id}
            </span>
          </span>
        </Table.Cell>
        {#each reportsFeatures as feature}
          <Table.Cell class="text-[#727272] h-full  w-[180px]">
            <Checkbox
              class="data-[state=checked]:bg-orange-500 data-[state=checked]:border-orange-500"
              checked={user?.features
                ? user?.features.includes(feature.id)
                : false}
              on:change={(e) =>
                handleFeatureChange(user.id, feature.id, e.target.checked)}
            />
          </Table.Cell>
        {/each}
      </Table.Row>
    {/each}
  </Table.Body>
  <Button on:click={handleFeaturesUpdate} class="mr-auto">Save</Button>
</Table.Root>
