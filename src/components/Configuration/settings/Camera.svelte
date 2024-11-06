<script lang="ts">
  //  import * as Select from "/components/ui/select";
  // import Icon from "@iconify/svelte";
  import * as Table from "@/components/ui/table";
  // import { onMount } from 'svelte';
  import { PenSquare } from "lucide-svelte";
  let logs = [];
  let allSelected = false;
  // import { Checkbox } from "$lib/components/ui/checkbox";

  onMount(() => {
    logs = Array.from({ length: 50 }, (_, i) => ({
      id: i + 1,
      errorType: ["Syntax Error", "Runtime Error", "Logical Error"][
        Math.floor(Math.random() * 3)
      ],
      severity: ["Critical", "Warning", "Informational"][
        Math.floor(Math.random() * 3)
      ],
      frequency: `5 Apr, 2024; ${12 + (i % 12)}:00 PM IST`,
      resolutionTime: "Solved within 2 hrs",
      errorTrends: "9 AM to 9 PM",
      rootCause: ["Recent code updates", "Database connection issues"][
        Math.floor(Math.random() * 2)
      ],
      selected: false,
    }));
  });

  let currentPage = 1;
  const rowsPerPage = 20;

  function toggleAll() {
    allSelected = !allSelected;
    logs.forEach((log) => (log.selected = allSelected));
  }

  $: allSelected = logs.every((log) => log.selected);

  function clearSelectedLogs() {
    logs = logs.filter((log) => !log.selected);
  }

  import PocketBase from "pocketbase";
  // import AddCameraDialog from "@/components/dialogs/AddCameraDialog.svelte";
  // import AddNodeDialog from "@/components/dialogs/AddNodeDialog.svelte";
  import AddNodeDialog from "@/components/ui/modal/AddNodeModal.svelte";
  // import AlertDeleteNode from "@/components/dialogs/alerts/AlertDeleteNode.svelte";
  import { selectedNode } from "@/stores";
  import type { User } from "@/types";
  import { Filter, Search, Trash2, X, Plus } from "lucide-svelte";
  export let user: User;
  import { onMount, onDestroy, getContext } from "svelte";
  import { toast } from "svelte-sonner";
  // import { addUserLog } from "@/lib/addUserLog";
  import { writable } from "svelte/store";
  // import { page } from "$app/stores";
  import * as Select from "@/components/ui/select";
  // import CameraDeleteDialog from "@/components/dialogs/CameraDeleteDialog.svelte";
  import { Input } from "@/components/ui/input";
  import Button from "@/components/ui/button/button.svelte";
  // import { cn } from "@/lib";
  import { buttonVariants } from "@/components/ui/button";
  import AddCameraModal from "@/components/ui/modal/AddCameraModal.svelte";

  let selected = 4;
  let detailIndex: number | null = null;
  let nodeIndex: number | null = null;
  let modify: boolean = false;
  let nodeModify: boolean = false;
  let newName: null | string = null;
  let searchNode = "";
  let newNodeName = "";
  let searchValue = "";
  let updatedPriority = null;
  let updatedFace = null;
  let updatedVehicle = null;
  let updatedSave = null;
  let updatedMotionThres = null;
  let filteredCameraNames = [];
  let filteredNodeNames = [];
  let enable = false;
  let newData = [];
  let view = 1;
  let change = "";
  let camStatusData = [];

  const PB: PocketBase = getContext("pb");

  const nodeData = writable([]);

  async function getNodeData() {
    if (user?.session) {
      const data = await fetch("/api/node/getMany", {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify({
          session: user?.session,
        }),
      });
      if (!data.ok) {
        toast.error("Something went wrong. Please try again");
        return;
      }
      const result = await data.json();
      nodeData.set(
        result?.nodeData.map((node) => ({ ...node, session: user.session })),
      );
    } else {
      console.log("no selected node");
    }
  }

  onMount(() => {
    getNodeData();

    // PB.collection("camera").subscribe("*", async (e) => {
    //   console.log("change", e.action, e.record);
    //   getNodeData();
    // });
  });

  onDestroy(() => {
    // PB.collection("camera").unsubscribe("*");
  });

  const onDeleteNode = async () => {
    const localCameraList =
      $nodeData[nodeIndex].camera.length === 0
        ? []
        : $nodeData[nodeIndex].camera;
    await fetch("/api/node/delete", {
      method: "delete",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify({
        cameras: $nodeData[nodeIndex].camera,
        nodeId: $nodeData[nodeIndex].id,
      }),
    }).then(() => {
      localCameraList.map((c) =>
        document.getElementById(`stream-${c.id}`)?.remove(),
      );
      toast(`Node: ${$nodeData[nodeIndex].name} deleted!`);
    });
  };

  $: {
    newData =
      $nodeData.length > 0
        ? $nodeData.filter((item: any) => {
            return item.session === user.session;
          })
        : [];
  }

  $: {
    if (searchNode.length === 0) {
      filteredNodeNames = [];
    }
    if (searchValue.length === 0) {
      filteredCameraNames = [];
      detailIndex = null;
    }
  }
</script>

<div
  class="flex flex-col overflow-y-scroll overflow-x-clip items-start pt-12 h-[calc(100vh-75px)] w-full text-black dark:text-white"
  id="camera"
>
  <!-- Selecting Tabs -->
  <div
    class="flex items-center justify-center rounded-lg border-black/[.13] border-solid border-[1px] p-1 w-[600px] max-h-[50px] h-[50px] mx-auto mb-10"
  >
    <button
      on:click={() => (selected = 1)}
      class={`disabled:cursor-not-allowed rounded-lg text-sm leading-[18px] px-[10px] py-2 font-medium w-1/2 h-full ${selected === 1 ? "text-white bg-[#015a62]" : "bg-transparent"}`}
      >Camera Details</button
    >
    <button
      on:click={() => (selected = 2)}
      class={`disabled:cursor-not-allowed rounded-lg text-sm leading-[18px] px-[10px] py-2 font-medium w-1/2 h-[40px] ${selected === 2 ? "text-white bg-[#015a62]" : "bg-transparent"}`}
      >Camera Status</button
    >
    <button
      on:click={() => (selected = 3)}
      class={`disabled:cursor-not-allowed rounded-lg text-sm leading-[18px] px-[10px] py-2 font-medium w-1/2 h-[40px] ${selected === 3 ? "text-white bg-[#015a62]" : "bg-transparent"}`}
      >Camera Failures</button
    >

    <button
      on:click={() => (selected = 4)}
      class={`disabled:cursor-not-allowed rounded-lg text-sm leading-[18px] px-[10px] py-2 font-medium w-1/2 h-full ${selected === 4 ? "text-white bg-[#015a62]" : "bg-transparent"}`}
      >Camera Logs</button
    >
  </div>

  <!-- <div
    class="flex w-full justify-start items-center gap-20 font-bold mb-4 px-8"
  >
    {#if selected === 1}
      <div class=" relative">
        <span class="font-bold text-[#015A62] dark:text-white">
          Camera details
        </span>
        <span
          class=" h-[3px] rounded-full bg-[#0B8995] w-full absolute left-0 -bottom-4"
        />
      </div>
    {:else}
      <button
        class="cursor-pointer text-[#212427] dark:text-[rgba(255,255,255,.6)] font-medium"
        on:click={() => {
          selected = 1;
          addUserLog("user clicked on camera details button Camera panel");
        }}
      >
        Camera details
      </button>
    {/if}
    {#if selected === 2}
      <div class=" relative">
        <span class="font-bold text-[#015A62] dark:text-white">
          Camera status
        </span>
        <span
          class=" h-[3px] rounded-full bg-[#0B8995] w-full absolute left-0 -bottom-4"
        />
      </div>
    {:else}
      <button
        class="cursor-pointer text-[#212427] dark:text-[rgba(255,255,255,.6)] font-medium"
        on:click={() => {
          selected = 2;
          addUserLog("user clicked on camera status button Camera panel");
        }}
      >
        Camera status
      </button>
    {/if}
    {#if selected === 3}
      <div class=" relative">
        <span class="font-bold text-[#015A62] dark:text-white">
          Camera failures
        </span>
        <span
          class=" h-[3px] rounded-full bg-[#0B8995] w-full absolute left-0 -bottom-4"
        />
      </div>
    {:else}
      <button
        class="cursor-pointer text-[#212427] dark:text-[rgba(255,255,255,.6)] font-medium"
        on:click={() => {
          selected = 3;
          addUserLog("user clicked on camera failures button Camera panel");
        }}
      >
        Camera failures
      </button>
    {/if}
    {#if selected === 4}
      <div class=" relative">
        <span class="font-bold text-[#015A62] dark:text-white">
          Camera logs
        </span>
        <span
          class=" h-[3px] rounded-full bg-[#0B8995] w-full absolute left-0 -bottom-4"
        />
      </div>
    {:else}
      <button
        class="cursor-pointer text-[#212427] dark:text-[rgba(255,255,255,.6)] font-medium"
        on:click={() => {
          selected = 4;
          addUserLog("user clicked on camera failures button Camera panel");
        }}
      >
        Camera logs
      </button>
    {/if}
  </div> -->

  <!-- camera details -->
  {#if selected === 1}
    <div
      class=" w-[94.5%] flex-col flex my-4 pt-8 shad mx-6 border-black/[.08] border-[1px] border-solid"
    >
      <h2 class="px-6 font-medium mb-4" id="details">Camera & Node Details</h2>
      <div
        class="h-[1px] dark:bg-[#292929] ml-2 mb-4 bg-[#e0e0e0] mt-8 w-[95%]"
      />
      <div class="flex flex-col items-start gap-2 px-6 mb-8">
        <span class="dark:text-[#e0e0e0] text-sm"> Node name or Id </span>
        <div class="flex items-center justify-center gap-5">
          <input
            autoComplete="off"
            placeholder="Search for node name or id "
            class="py-4 w-[330px] pl-3 rounded-md border border-solid border-[#979797] active:border-[#015a62] text-xs text-[#979797] dark:bg-[#242424] dark:border-[#333] dark:text-[#929292]"
            bind:value={searchNode}
            on:input={(e) => {
              searchNode = e.target.value;
              let filteredNodes = newData.filter((node) => {
                return node.name
                  .toLowerCase()
                  .includes(e.target.value.toLowerCase());
              });
              filteredNodeNames = [...filteredNodes];
              let exact = newData.filter((node) => {
                return node.name.toLowerCase() === e.target.value.toLowerCase();
              });
              if (exact.length > 0) {
                enable = true;
              } else {
                enable = false;
              }
            }}
          />
          <button
            disabled={!enable}
            class="disabled:cursor-not-allowed py-2.5 px-5 text-sm hover:bg-[#015A62] dark:hover:bg-[#015a62] dark:bg-[#828282] dark:text-white rounded-md text-[#4f4f4f] hover:text-white text-[4f4f4f] bg-[#d9d9d9]"
            on:click={() => (nodeIndex = 0)}
          >
            Modify
          </button>
        </div>
      </div>
      <div
        class="flex flex-col items-center max-w-[60%] ml-6 justify-center w-full dark:border-[#939393] border-solid border mb-10"
      >
        <div
          class=" dark:bg-[#1b1b1b] bg-white px-6 w-full flex items-center justify-between py-2"
        >
          <span class="font-semibold">Node</span>
          <div class="flex gap-4">
            <AddNodeDialog {user} {getNodeData}>
              <Button
                variant="outline"
                size="sm"
                class={"border-none flex text-xs items-center gap-2 font-medium rounded-3xl border text-primary border-green-200 hover:bg-green-100 bg-[#01666717]/10"}
              >
                <Plus class="w-3.5 h-3.5 text-primary" />
                Add
              </Button>
            </AddNodeDialog>
            {#if !nodeModify}
              <Button
                variant="outline"
                size="sm"
                class={"border-none flex text-xs items-center gap-2 font-medium rounded-3xl border text-orange-500 bg-orange-50 hover:bg-orange-100"}
                on:click={() => {
                  if (nodeIndex !== undefined && nodeIndex !== null) {
                    nodeModify = true;
               
                  } else {
                    alert("please select a node");
                
                  }
                }}
              >
                <PenSquare class="w-3.5 h-3.5" />
                Modify
              </Button>
            {:else}
              <button
                class="dark:text-[#cac4d0] text-[#4f4f4f] font-xs bg-[#d9d9d9] border-none dark:bg-[#242424] rounded-md px-3 py-1.5 border-[#333] border dark:border-solid"
                on:click={async () => {
                  await fetch("/api/node/update", {
                    method: "PATCH",
                    headers: {
                      "Content-Type": "application/json",
                    },
                    body: JSON.stringify({
                      name: newNodeName,
                      id: newData[nodeIndex].id,
                    }),
                  }).catch((err) => {
                    toast.error("failed to update node name");
                    return;
                  });
                  toast(
                    `${newData[nodeIndex].name} updated to ${newNodeName}!`,
                  );
                  newData[nodeIndex].name = newNodeName;
                  nodeModify = false;
                }}
              >
                Save Changes
              </button>
            {/if}
            {#if nodeModify}
              <button
                class="dark:text-[#cac4d0] text-[#4f4f4f] font-xs bg-[#d9d9d9] border-none dark:bg-[#242424] rounded-md px-3 py-1.5 border-[#333] border dark:border-solid"
                on:click={() => (nodeModify = false)}
              >
                Discard Changes
              </button>
            {/if}
            <!-- <AlertDeleteNode onDelete={onDeleteNode}> -->
              <Button
                disabled={nodeIndex === null}
                variant="outline"
                size="sm"
                class={"disabled:cursor-not-allowed border-none flex text-xs items-center gap-2 font-medium rounded-3xl border text-red-500 border-red-200 hover:bg-red-100 bg-red-50"}
                on:click={() => {
                  if (nodeIndex !== undefined && nodeIndex !== null) {
                    nodeModify = true;
                 
                  } else {
                    alert("please select a node");
               
                  }
                }}
              >
                <Trash2 class="w-3.5 h-3.5" />
                Remove
              </Button>
            <!-- </AlertDeleteNode> -->
          </div>
        </div>

        <table class="w-full bg-white shadow-md rounded">
          <tbody>
            {#if filteredNodeNames.length > 0}
              {#each filteredNodeNames as item, index}
                <tr
                  class={`dark:bg-[#1b1b1b] bg-[white] cursor-pointer border-b-[1px] border-solid dark:border-[#929292] ${
                    index === 0 ? "border-t-[1px]" : ""
                  } `}
                  on:click={async () => {
                    if (nodeIndex === index) {
                      if (!nodeModify) {
                        nodeIndex = null;
                      }
                    } else {
                      nodeIndex = index;
                      // console.log(filteredNodeNames[index].id);
                      // const status = await PB.collection(
                      //   "camera_ping_status",
                      // ).getList(1, 100, {
                      //   filter: `node~"${filteredNodeNames[index].id}"`,
                      //   sort: "-created",
                      // });
                      // console.log("status", status);
                      // // camStatusData = [...new Set(status.items.map(item => item.url))];
                      // // camStatusData = [...new Set(status.items.map(item => item))];
                      // const uniqueUrls = new Set();
                      // const uniqueStatus = status.items.filter((item) => {
                      //   if (!uniqueUrls.has(item.url)) {
                      //     uniqueUrls.add(item.url);
                      //     return true;
                      //   }
                      //   return false;
                      // });
                      // camStatusData = uniqueStatus;
                    }
                  }}
                >
                  <td
                    class=" text-center py-2 px-4 border-r-[1px] border-solid dark:border-[#929292] w-1/2"
                  >
                    {#if nodeModify && index === nodeIndex}
                      <input
                        type="text"
                        autoComplete="off"
                        name="node-name"
                        placeholder={item.name}
                        bind:value={newNodeName}
                        on:input={(e) => {
                          console.log(e.target.value);
                          newNodeName = e.target.value;
                        }}
                        class="block border-0 px-1 py-1 text-gray-900
                    placeholder:text-gray-400
                      bg-transparent w-full
                      focus:border-b-2
                    focus:border-indigo-600
                    dark:text-[#979797] sm:text-sm sm:leading-6"
                      />
                    {:else}
                      {item.name}
                    {/if}
                  </td>
                  <td class="text-center py-3 px-4 h-full w-1/2">
                    <div class="flex justify-center items-center h-full">
                      <input
                        class="w-auto cursor-pointer"
                        type="checkbox"
                        checked={nodeIndex === index}
                      />
                    </div>
                  </td>
                </tr>
              {/each}
            {:else}
              {#each $nodeData as data, index}
                <tr
                  class={`dark:bg-[#1b1b1b] bg-[white] cursor-pointer border-b-[1px] border-solid dark:border-[#929292] ${
                    index === 0 ? "border-t-[1px]" : ""
                  } `}
                  on:click={async () => {
                    if (nodeIndex === index) {
                      if (!nodeModify) {
                        nodeIndex = null;
                      }
                    } else {
                      nodeIndex = index;
                      console.log($nodeData[index].id);
                      // const status = await PB.collection(
                      //   "camera_ping_status",
                      // ).getList(1, 100, {
                      //   filter: `node~"${$nodeData[index].id}"`,
                      //   sort: "-created",
                      // });
                      // console.log("status", status);
                      // // camStatusData = [...new Set(status.items.map(item => item.url))];
                      // // camStatusData = [...new Set(status.items.map(item => item))];
                      // const uniqueUrls = new Set();
                      // const uniqueStatus = status.items.filter((item) => {
                      //   if (!uniqueUrls.has(item.url)) {
                      //     uniqueUrls.add(item.url);
                      //     return true;
                      //   }
                      //   return false;
                      // });
                      // camStatusData = uniqueStatus;
                    }
                  }}
                >
                  <td
                    class=" text-center py-2 px-4 border-r-[1px] border-solid dark:border-[#929292] w-1/2"
                  >
                    {#if nodeModify && index === nodeIndex}
                      <input
                        type="text"
                        autoComplete="off"
                        name="node-name"
                        placeholder={data.name}
                        bind:value={newNodeName}
                        on:input={(e) => {
                          console.log(e.target.value);
                          newNodeName = e.target.value;
                        }}
                        class="block border-0 px-1 py-1 text-gray-900
                        placeholder:text-gray-400
                          bg-transparent w-full
                          focus:border-b-2
                        focus:border-indigo-600
                        dark:text-[#979797] sm:text-sm sm:leading-6"
                      />
                    {:else}
                      {data.name}
                    {/if}
                  </td>
                  <td class="text-center py-3 px-4 h-full w-1/2">
                    <div class="flex justify-center items-center h-full">
                      <input
                        class="w-auto cursor-pointer"
                        type="checkbox"
                        checked={nodeIndex === index}
                      />
                      <!-- on:click={() => {
                          console.log($selectedNode);
                          const foundNode = nodeD.find(
                            (node) => node.id === data.id,
                          );
                          console.log("Found node:", foundNode);
                        }} -->
                    </div>
                  </td>
                </tr>
              {/each}
            {/if}
          </tbody>
        </table>
      </div>

      {#if nodeIndex !== undefined && nodeIndex !== null}
        <span class="px-6 text-sm dark:text-[#e0e0e0] pb-4">
          Camera Details
        </span>
        <div
          class="flex flex-col items-center max-w-[96%] mx-auto justify-center w-full dark:border-[#939393] border-solid border mb-20"
        >
          <div
            class="dark:bg-[#1b1b1b] py-2 flex w-full items-center justify-between px-6"
          >
            <div class="relative">
              <input
                placeholder="Search camera by name"
                class="pr-2 pl-10 py-1 my-1 rounded-md border border-solid dark:border-[#333] dark:bg-[#242424] w-[300px] placeholder-opacity-70"
                bind:value={searchValue}
                on:input={(e) => {
                  searchValue = e.target.value;
                  let filteredNames = newData[nodeIndex].cameraData.filter(
                    (camera) => {
                      return camera[0].name
                        .toLowerCase()
                        .includes(e.target.value.toLowerCase());
                    },
                  );
                  filteredCameraNames = [...filteredNames];
                }}
              />
              <Search class="absolute top-1/2 -translate-y-1/2 left-2" />
              <button
                class="absolute top-1/2 -translate-y-1/2 right-2"
                on:click={() => (searchValue = "")}
              >
                <X />
              </button>
            </div>
            <div class="flex gap-4">
              <AddCameraDialogTech sNode={newData[nodeIndex]}>
                <Button
                  variant="outline"
                  size="sm"
                  class={"border-none flex text-xs items-center gap-2 font-medium rounded-3xl border text-primary border-green-200 hover:bg-green-100 bg-[#01666717]/10"}
                >
                  <Plus class="w-3.5 h-3.5 text-primary" />
                  Add
                </Button>
              </AddCameraDialogTech>
              <!-- <AddCameraDialog sNode={newData[nodeIndex]}>
                <Button
                  variant="outline"
                  size="sm"
                  class={"border-none flex text-xs items-center gap-2 font-medium rounded-3xl border text-primary border-green-200 hover:bg-green-100 bg-[#01666717]/10"}
                  on:click={() =>
                    addUserLog(
                      `user clicked on add camera button camera table`,
                    )}
                >
                  <Plus class="w-3.5 h-3.5 text-primary" />
                  Add
                </Button>
              </AddCameraDialog> -->
              {#if !modify}
                <Button
                  variant="outline"
                  size="sm"
                  class={"border-none flex text-xs items-center gap-2 font-medium rounded-3xl border text-orange-500 bg-orange-50 hover:bg-orange-100"}
                  on:click={() => {
                    if (detailIndex !== undefined && detailIndex !== null) {
                      modify = true;
                    } else {
                      alert("please select a camera");
                    }
                  }}
                >
                  <PenSquare class="w-3.5 h-3.5" />
                  Modify
                </Button>
              {:else}
                <button
                  class="dark:text-[#cac4d0] text-[#4f4f4f] font-xs bg-[#d9d9d9] border-none dark:bg-[#242424] rounded-md px-3 py-1.5 border-[#333] border dark:border-solid"
                  on:click={() => {
                    fetch("/api/camera/editCamera", {
                      method: "put",
                      headers: {
                        "Content-Type": "application/json",
                      },
                      body: JSON.stringify({
                        cameraId: newData[nodeIndex].cameraData[0][0].id,
                        nodeId: newData[nodeIndex].id,
                        name:
                          newName ?? newData[nodeIndex].cameraData[0][0].name,
                        url: newData[nodeIndex].cameraData[0][0].url,
                        priority:
                          updatedPriority ??
                          newData[nodeIndex].cameraData[0][0].priority,
                        face:
                          updatedFace ??
                          newData[nodeIndex].cameraData[0][0].face,
                        vehicle:
                          updatedVehicle ??
                          newData[nodeIndex].cameraData[0][0].vehicle,
                        save:
                          updatedSave ??
                          newData[nodeIndex].cameraData[0][0].save,
                        motionThresh:
                          updatedMotionThres ??
                          newData[nodeIndex].cameraData[0][0].motionThresh,
                      }),
                    }).then(() => {
                      toast("Camera updated!");
                      modify = false;
                    });
                  }}
                >
                  Save Changes
                </button>
              {/if}
              {#if modify}
                <button
                  class="dark:text-[#cac4d0] text-[#4f4f4f] font-xs bg-[#d9d9d9] border-none dark:bg-[#242424] rounded-md px-3 py-1.5 border-[#333] border dark:border-solid"
                  on:click={() => {
                    modify = false;
                  }}
                >
                  Discard
                </button>
              {/if}
              <!-- <CameraDeleteDialog
                cameraId={newData?.[nodeIndex]?.cameraData?.[detailIndex]?.[0]
                  ?.id}
                nodeId={newData?.[nodeIndex]?.id}
                url={newData?.[nodeIndex]?.cameraData?.[detailIndex]?.[0]?.url}
                name={newData?.[nodeIndex]?.cameraData?.[detailIndex]?.[0]
                  ?.name}
              > -->
                <Button
                  disabled={detailIndex === null}
                  variant="outline"
                  size="sm"
                  class={"disabled:cursor-not-allowed border-none flex text-xs items-center gap-2 font-medium rounded-3xl border text-red-500 border-red-200 hover:bg-red-100 bg-red-50"}
                  on:click={() => {
                    if (nodeIndex !== undefined && nodeIndex !== null) {
                      nodeModify = true;
                    } else {
                      alert("please select a node");
                     
                    }
                  }}
                >
                  <Trash2 class="w-3.5 h-3.5" />
                  Remove
                </Button>
              <!-- </CameraDeleteDialog> -->
            </div>
          </div>
          <table class="w-full bg-white shadow-md rounded">
            <thead>
              <tr
                class="dark:bg-[#1b1b1b] bg-[#f2f2f2] border-t-[1px] border-solid dark:border-[#929292]"
              >
                <th
                  class="text-left py-3 px-4 flex items-center justify-center"
                >
                  Select
                </th>
                <th
                  class="text-left py-3 px-4 border-x-[1px] border-solid dark:border-[#929292]"
                >
                  Node
                </th>
                <th
                  class="text-left py-3 px-4 border-r-[1px] border-solid dark:border-[#929292]"
                >
                  Camera name
                </th>
                <th
                  class="text-left py-3 px-4 border-r-[1px] border-solid dark:border-[#929292]"
                >
                  Camera Id
                </th>
                <th
                  class="text-left py-3 px-4 border-r-[1px] border-solid dark:border-[#929292]"
                >
                  IP Address
                </th>
                <th
                  class="text-left py-3 px-4 border-r-[1px] border-solid dark:border-[#929292]"
                >
                  Protocol
                </th>
                <th
                  class="text-left py-3 px-4 border-r-[1px] border-solid dark:border-[#929292]"
                >
                  Priority
                </th>
                <th
                  class="text-left py-3 px-4 border-r-[1px] border-solid dark:border-[#929292]"
                >
                  Motion Thresh
                </th>
                <th
                  class="text-left py-3 px-4 border-r-[1px] border-solid dark:border-[#929292]"
                >
                  Face
                </th>
                <th
                  class="text-left py-3 px-4 border-r-[1px] border-solid dark:border-[#929292]"
                >
                  Vehicle
                </th>
                <th class="text-left py-3 px-4">Save</th>
              </tr>
            </thead>
            <tbody>
              {#if filteredCameraNames.length > 0}
                {#each filteredCameraNames as item, index}
                  <tr
                    class={`${
                      index % 2 === 0
                        ? "dark:bg-[#333] bg-white cursor-pointer"
                        : "dark:bg-[#242424] bg-[#f2f2f2] cursor-pointer"
                    } ${
                      index === filteredCameraNames.length - 1
                        ? "border-b-[1px] border-solid dark:border-[#929292]"
                        : ""
                    } `}
                    on:click={() => {
                      if (detailIndex === index) {
                        detailIndex = null;
                      } else {
                        detailIndex = index;
                      }
                    }}
                  >
                    <td
                      class="text-left py-3 px-4 h-full flex items-center justify-center cursor-pointer"
                    >
                      <input
                        checked={detailIndex === index}
                        class=" cursor-pointer"
                        type="checkbox"
                      />
                    </td>
                    <td
                      class="py-2 px-4 border-x-[1px] border-solid dark:border-[#929292]"
                    >
                      {newData[nodeIndex].name}
                    </td>
                    <td
                      class="py-2 px-4 border-r-[1px] border-solid dark:border-[#929292]"
                    >
                      {#if modify && index === detailIndex}
                        <input
                          type="text"
                          name="node-name"
                          autoComplete="off"
                          placeholder={item?.name}
                          class="block border-0 px-1 py-1 text-gray-900
                  placeholder:text-gray-400
                    bg-transparent w-full
                    focus:border-b-2
                  focus:border-indigo-600
                  dark:text-[#979797] sm:text-sm sm:leading-6"
                          bind:value={newName}
                          on:input={(e) => {
                            newName = e.target.value;
                          }}
                        />
                      {:else}
                        {item[0].name}
                      {/if}
                    </td>
                    <td
                      class="py-2 px-4 border-r-[1px] border-solid dark:border-[#929292]"
                    >
                      {item[0].id}
                    </td>
                    <td
                      class="py-2 px-4 border-r-[1px] border-solid dark:border-[#929292]"
                    >
                      {item?.[0].url.split("@")[1].split(":")[0]}
                    </td>
                    <td
                      class="py-2 px-4 border-r-[1px] border-solid dark:border-[#929292]"
                    >
                      {item?.[0].url.slice(0, 4)}
                    </td>
                    <td
                      class="py-2 px-4 border-r-[1px] border-solid dark:border-[#929292]"
                    >
                      {#if modify && index === detailIndex}
                        <Select.Root
                          portal={null}
                          onSelectedChange={(e) => (updatedPriority = e?.value)}
                        >
                          <Select.Trigger class="text-sm ">
                            class='pl-6'
                            <Select.Value placeholder={item[0].priority} />
                          </Select.Trigger>
                          <Select.Content>
                            <Select.Group>
                              <Select.Item class="pl-6" value={0} label="0"
                                >0</Select.Item
                              >
                              <Select.Item class="pl-6" value={1} label="1"
                                >1</Select.Item
                              >
                            </Select.Group>
                          </Select.Content>
                        </Select.Root>
                      {:else}
                        {item?.[0].priority}{/if}
                    </td>
                    <td
                      class="py-2 px-4 border-r-[1px] border-solid dark:border-[#929292]"
                    >
                      {#if modify && index === detailIndex}
                        <Select.Root
                          portal={null}
                          onSelectedChange={(e) =>
                            (updatedMotionThres = e?.value)}
                        >
                          <Select.Trigger class="text-sm ">
                            <Select.Value
                              placeholder={item?.[0].motionThresh === 5000
                                ? "High"
                                : item?.[0].motionThresh === "2500"
                                  ? "Mid"
                                  : "Low"}
                            />
                          </Select.Trigger>
                          <Select.Content>
                            <Select.Group>
                              <Select.Item value={1000} label={"low"}
                                >Low</Select.Item
                              >
                              <Select.Item value={2500} label={"mid"}
                                >Mid</Select.Item
                              >
                              <Select.Item value={5000} label={"high"}
                                >High</Select.Item
                              >
                            </Select.Group>
                          </Select.Content>
                        </Select.Root>
                      {:else}
                        {item?.[0].motionThresh === 5000
                          ? "High"
                          : item?.[0].motionThresh === "2500"
                            ? "Mid"
                            : "Low"}
                      {/if}
                    </td>
                    <td
                      class="py-2 px-4 border-r-[1px] border-solid dark:border-[#929292]"
                    >
                      {#if modify && index === detailIndex}
                        <Select.Root
                          portal={null}
                          onSelectedChange={(e) => (updatedFace = e?.value)}
                        >
                          <Select.Trigger class="text-sm ">
                            <Select.Value placeholder={item[0].face} />
                          </Select.Trigger>
                          <Select.Content>
                            <Select.Group>
                              <Select.Item
                                value={true}
                                label={"true"}
                                class="pl-6">True</Select.Item
                              >
                              <Select.Item
                                value={false}
                                label={"False"}
                                class="pl-6">False</Select.Item
                              >
                            </Select.Group>
                          </Select.Content>
                        </Select.Root>
                      {:else}
                        {item?.[0].face}
                      {/if}
                    </td>
                    <td
                      class="py-2 px-4 border-r-[1px] border-solid dark:border-[#929292]"
                    >
                      {#if modify && index === detailIndex}
                        <Select.Root
                          portal={null}
                          onSelectedChange={(e) => (updatedVehicle = e?.value)}
                        >
                          <Select.Trigger class="text-sm ">
                            <Select.Value placeholder={item[0].vehicle} />
                          </Select.Trigger>
                          <Select.Content>
                            <Select.Group>
                              <Select.Item
                                value={true}
                                label={"true"}
                                class="pl-6">True</Select.Item
                              >
                              <Select.Item
                                value={false}
                                label={"False"}
                                class="pl-6">False</Select.Item
                              >
                            </Select.Group>
                          </Select.Content>
                        </Select.Root>
                      {:else}
                        {item?.[0].vehicle}
                      {/if}
                    </td>
                    <td class="py-2 px-4">
                      {#if modify && index === detailIndex}
                        <Select.Root
                          portal={null}
                          onSelectedChange={(e) => (updatedSave = e?.value)}
                        >
                          <Select.Trigger class="text-sm ">
                            <Select.Value placeholder={item[0].save} />
                          </Select.Trigger>
                          <Select.Content>
                            <Select.Group>
                              <Select.Item
                                value={true}
                                label={"true"}
                                class="pl-6">True</Select.Item
                              >
                              <Select.Item
                                value={false}
                                label={"False"}
                                class="pl-6">False</Select.Item
                              >
                            </Select.Group>
                          </Select.Content>
                        </Select.Root>
                      {:else}{item[0].save}{/if}</td
                    >
                  </tr>
                {/each}
              {:else if newData && newData?.[nodeIndex]?.camera?.length > 0}
                {#each newData?.[nodeIndex].cameraData as item, index}
                  <tr
                    class={`${
                      index % 2 === 0
                        ? "dark:bg-[#333] bg-white cursor-pointer"
                        : "dark:bg-[#242424] bg-[#f2f2f2] cursor-pointer"
                    } ${
                      index === newData?.[nodeIndex].camera.length - 1
                        ? "border-b-[1px] border-solid dark:border-[#929292]"
                        : ""
                    } 
            `}
                    on:click={() => {
                      if (detailIndex === index && !modify) {
                        detailIndex = null;
                      } else {
                        detailIndex = index;
                      }
                    }}
                  >
                    <td
                      class="text-left py-3 px-4 h-full flex items-center justify-center"
                    >
                      <input
                        type="checkbox"
                        class="cursor-pointer"
                        checked={detailIndex === index}
                      />
                    </td>
                    <td
                      class="py-2 px-4 border-x-[1px] border-solid dark:border-[#929292]"
                    >
                      {newData[nodeIndex].name}
                    </td>

                    <td
                      class="py-2 px-4 border-r-[1px] border-solid dark:border-[#929292]"
                    >
                      {#if modify && index === detailIndex}
                        <input
                          type="text"
                          name="node-name"
                          autoComplete="off"
                          placeholder={item[0]?.name}
                          class="block border-0 px-1 py-1 text-gray-900
                  placeholder:text-gray-400
                    bg-transparent w-full
                    focus:border-b-2
                  focus:border-indigo-600
                  dark:text-[#979797] sm:text-sm sm:leading-6"
                          bind:value={newName}
                          on:input={(e) => {
                            newName = e.target.value;
                          }}
                        />
                      {:else}
                        {item[0].name}
                      {/if}
                    </td>

                    <td
                      class="py-2 px-4 border-r-[1px] border-solid dark:border-[#929292]"
                      >{item[0].id}</td
                    >
                    <td
                      class="py-2 px-4 border-r-[1px] border-solid dark:border-[#929292]"
                      >{item[0].url.split("@")[1]?.split(":")[0]}</td
                    >
                    <td
                      class="py-2 px-4 border-r-[1px] border-solid dark:border-[#929292]"
                    >
                      {item?.[0].url.slice(0, 4)}</td
                    >
                    <td
                      class="py-2 px-4 border-r-[1px] border-solid dark:border-[#929292]"
                    >
                      {#if modify && index === detailIndex}
                        <Select.Root
                          portal={null}
                          onSelectedChange={(e) => (updatedPriority = e?.value)}
                        >
                          <Select.Trigger class="text-sm ">
                            <Select.Value placeholder={item[0].priority} />
                          </Select.Trigger>
                          <Select.Content>
                            <Select.Group>
                              <Select.Item class="pl-6" value={0} label={"0"}
                                >0</Select.Item
                              >
                              <Select.Item class="pl-6" value={1} label={"1"}
                                >1</Select.Item
                              >
                            </Select.Group>
                          </Select.Content>
                        </Select.Root>
                      {:else}
                        {item?.[0].priority}{/if}</td
                    >
                    <td
                      class="py-2 px-4 border-r-[1px] border-solid dark:border-[#929292]"
                    >
                      {#if modify && index === detailIndex}
                        <Select.Root
                          portal={null}
                          onSelectedChange={(e) =>
                            (updatedMotionThres = e?.value)}
                        >
                          <Select.Trigger class="text-sm ">
                            <Select.Value
                              placeholder={item?.[0].motionThresh === 5000
                                ? "High"
                                : item?.[0].motionThresh === "2500"
                                  ? "Mid"
                                  : "Low"}
                            />
                          </Select.Trigger>
                          <Select.Content>
                            <Select.Group>
                              <Select.Item value={1000} label={"low"}
                                >Low</Select.Item
                              >
                              <Select.Item value={2500} label={"mid"}
                                >Mid</Select.Item
                              >
                              <Select.Item value={5000} label={"high"}
                                >High</Select.Item
                              >
                            </Select.Group>
                          </Select.Content>
                        </Select.Root>
                      {:else}
                        {item?.[0].motionThresh === 5000
                          ? "High"
                          : item?.[0].motionThresh === "2500"
                            ? "Mid"
                            : "Low"}
                      {/if}
                    </td>

                    <td
                      class="py-2 px-4 border-r-[1px] border-solid dark:border-[#929292]"
                    >
                      {#if modify && index === detailIndex}
                        <Select.Root
                          portal={null}
                          onSelectedChange={(e) => (updatedFace = e?.value)}
                        >
                          <Select.Trigger class="text-sm ">
                            <Select.Value placeholder={item[0].face} />
                          </Select.Trigger>
                          <Select.Content>
                            <Select.Group>
                              <Select.Item
                                value={true}
                                label={"true"}
                                class="pl-6">True</Select.Item
                              >
                              <Select.Item
                                value={false}
                                label={"False"}
                                class="pl-6">False</Select.Item
                              >
                            </Select.Group>
                          </Select.Content>
                        </Select.Root>
                      {:else}
                        {item?.[0].face}
                      {/if}
                    </td>

                    <td
                      class="py-2 px-4 border-r-[1px] border-solid dark:border-[#929292]"
                    >
                      {#if modify && index === detailIndex}
                        <Select.Root
                          portal={null}
                          onSelectedChange={(e) => (updatedVehicle = e?.value)}
                        >
                          <Select.Trigger class="text-sm ">
                            <Select.Value placeholder={item[0].vehicle} />
                          </Select.Trigger>
                          <Select.Content>
                            <Select.Group>
                              <Select.Item
                                value={true}
                                label={"true"}
                                class="pl-6">True</Select.Item
                              >
                              <Select.Item
                                value={false}
                                label={"False"}
                                class="pl-6">False</Select.Item
                              >
                            </Select.Group>
                          </Select.Content>
                        </Select.Root>
                      {:else}
                        {item[0].vehicle}
                      {/if}</td
                    >
                    <td
                      class="py-2 px-4 border-r-[1px] border-solid dark:border-[#929292]"
                    >
                      {#if modify && index === detailIndex}
                        <Select.Root
                          portal={null}
                          onSelectedChange={(e) => (updatedSave = e?.value)}
                        >
                          <Select.Trigger class="text-sm ">
                            <Select.Value placeholder={item[0].save} />
                          </Select.Trigger>
                          <Select.Content>
                            <Select.Group>
                              <Select.Item
                                value={true}
                                label={"true"}
                                class="pl-6">True</Select.Item
                              >
                              <Select.Item
                                value={false}
                                label={"False"}
                                class="pl-6">False</Select.Item
                              >
                            </Select.Group>
                          </Select.Content>
                        </Select.Root>
                      {:else}{item[0].save}{/if}</td
                    >
                  </tr>
                {/each}
              {/if}
            </tbody>
          </table>
        </div>
      {:else}
        <span class="px-6 text-sm dark:text-[#e0e0e0] mb-8">
          Select a node to get its camera details.
        </span>
      {/if}
    </div>
  {/if}

  <!-- camera status -->
  {#if selected === 2}
    <div
      class=" w-[94.5%] my-4 pt-8 shad mx-6 border-black/[.08] border-[1px] border-solid flex flex-col"
    >
      <h2 class="px-6 font-medium mb-4" id="status">
        Camera Status{" "}
      </h2>
      <div
        class="h-[1px] dark:bg-[#292929] ml-2 mb-4 bg-[#e0e0e0] mt-8 w-[95%]"
      />
      {#if nodeIndex !== undefined && nodeIndex !== null}
        <div
          class="flex flex-col items-center max-w-[96%] mx-auto justify-center w-full dark:border-[#939393] border-solid border mb-20"
        >
          <div class="dark:bg-[#1e1f25] py-2 w-full px-6" />
          <table class="w-full bg-white] shadow-md rounded">
            <thead>
              <tr
                class="dark:bg-[#1a1a1a] bg-[#f2f2f2] border-t-[1px] border-solid dark:border-[#929292]"
              >
                <th
                  class="text-left py-3 px-4 border-r-[1px] border-solid dark:border-[#929292]"
                >
                  Camera name
                </th>
                <th
                  class="text-left py-3 px-4 border-r-[1px] border-solid dark:border-[#929292]"
                >
                  Node
                </th>
                <th
                  class="text-left py-3 px-4 border-r-[1px] border-solid dark:border-[#929292]"
                >
                  Ip Address
                </th>
                <th
                  class="text-left py-3 px-4 border-r-[1px] border-solid dark:border-[#929292]"
                >
                  Connection Status
                </th>
                <th
                  class="text-left py-3 px-4 border-r-[1px] border-solid dark:border-[#929292]"
                >
                  Signal Strength
                </th>
                <th class="text-left py-3 px-4">Failures</th>
              </tr>
            </thead>
            <tbody>
              {#if newData}
                {#each newData?.[nodeIndex].cameraData as item, index}
                  <tr
                    class={`${
                      index % 2 === 0
                        ? "dark:bg-[#333] bg-white cursor-pointer"
                        : "dark:bg-[#242424] bg-[#f2f2f2] cursor-pointer"
                    } ${
                      index === newData?.[nodeIndex].camera.length - 1
                        ? "border-b-[1px] border-solid dark:border-[#929292]"
                        : ""
                    }`}
                  >
                    <td
                      class="py-2 px-4 border-r-[1px] border-solid dark:border-[#929292]"
                    >
                      {item[0]?.name}
                    </td>
                    <td
                      class="py-2 px-4 border-r-[1px] border-solid dark:border-[#929292]"
                    >
                      {newData[nodeIndex].name}
                    </td>
                    <td
                      class="py-2 px-4 border-r-[1px] border-solid dark:border-[#929292]"
                    >
                      {item[0].url?.split("@")[1]?.split(":")[0]}
                    </td>
                    <td
                      class="py-2 px-4 border-r-[1px] border-solid dark:border-[#929292]"
                    >
                      <!-- Status  -->
                      {camStatusData.find(
                        (status) => status.camera === item[0].id,
                      )?.status
                        ? "Online"
                        : "Offline" || "Unknown"}
                    </td>
                    <td
                      class="py-2 px-4 border-r-[1px] border-solid dark:border-[#929292]"
                    >
                      Strength
                    </td>
                    <td class="py-2 px-4">failures</td>
                  </tr>
                {/each}
              {/if}
            </tbody>
          </table>
          <div
            class=" dark:bg-[#1e1f25] bg-white px-6 w-full flex items-center justify-end py-4"
          />
        </div>
      {:else}
        <span class="px-6 text-sm dark:text-[#e0e0e0] mb-4">
          Select a node to get its camera details.
        </span>
      {/if}
    </div>
  {/if}

  <!-- camera failures -->
  {#if selected === 3}
    <div
      class=" w-[94.5%] my-4 pt-8 shad mx-6 border-black/[.08] border-[1px] border-solid flex flex-col"
    >
      <h2 class="px-6 font-medium mb-4" id="fail">
        Camera Failures{" "}
      </h2>
      <div
        class="h-[1px] dark:bg-[#292929] ml-2 mb-4 bg-[#e0e0e0] mt-8 w-[95%]"
      />
      <p class="px-6 text-sm dark:text-[#929292] mb-8">
        All details when camera was not working properly will be shown here.
      </p>
    </div>
  {/if}

  <!-- camera logs -->
  {#if selected === 4}
    <section class=" w-[94.5%] my-4 shad mx-6 flex flex-col">
      <div class="flex items-center justify-between px-4 w-[65%]">
        <div class="left flex flex-col gap-1">
          <div class="flex items-center gap-2 mb-1">
            <h2 class="font-medium text-xl text-[#323232]">
              {view === 1 ? "Camera" : "Node"} Logs
            </h2>
            <span
              class="text-sm text-black/.5 bg-[#F7FAFF] rounded-lg px-2 py-1 text-[#0070FF]"
              >Super Admin</span
            >
          </div>
          <h5 class="text-sm text-neutral-500">
            Complete logs from all the {view === 1 ? "cameras" : "nodes"}
          </h5>
        </div>
        <div
          class="flex items-center justify-center rounded-lg border-black/[.13] dark:border-white/[.13] border-solid border-[1px] p-1 w-[300px] h-[40px] mt-4"
        >
          <button
            on:click={() => (view = 1)}
            class={`rounded-lg text-xs leading-[18px] px-[10px] py-[3px] font-medium w-1/2 h-full ${view === 1 ? "text-white bg-[#015a62]" : "bg-transparent"}`}
            >Cameras</button
          >
          <button
            on:click={() => (view = 2)}
            class={`rounded-lg text-xs leading-[18px] px-[10px] py-[3px] font-medium w-1/2 h-full ${view === 2 ? "text-white bg-[#015a62]" : "bg-transparent"}`}
            >Node</button
          >
        </div>
      </div>
      {#if view === 1}
        <div class="flex flex-col gap-4 p-4 mt-8">
          <div class="flex flex-row justify-between items-center">
            <div class="flex items-center gap-5">
              <span
                class="text-[#3c3c3c] font-semibold p-2 rounded-md bg-[#F1f1f1]"
                >Total Cameras: $$</span
              >
              <span class="text-[#6B6B6B] font-semibold">Not Started: $$</span>
              <span class="text-[#F16522] font-semibold">Faulty $$</span>
              <span class="text-[#016667] font-semibold">Running: $$</span>
              <span class="text-[#F93333] font-semibold">Not Working: $$</span>
            </div>
            <div class="flex items-center gap-5">
              <button class="flex items-center gap-2"
                ><Filter size={20} /> Filters</button
              >
              <span class="relative">
                <Input placeholder="Search" class="w-[250px] pl-8" />
                <Search
                  size={18}
                  class="absolute top-1/2 -translate-y-1/2 left-2"
                />
              </span>
            </div>
          </div>
          <Table.Root>
            <Table.Header
              class="bg-[#F9f9f9] rounded-md border border-solid border-[#e4e4e4]"
            >
              <Table.Row class="bg-transparent border-b-0 w-full">
                <Table.Head>Camera Name</Table.Head>
                <Table.Head>Location</Table.Head>
                <Table.Head>Uptime (%)</Table.Head>
                <Table.Head>Downtime (%)</Table.Head>
                <Table.Head>Last Maintenance</Table.Head>
                <Table.Head>Storage Usage (GB)</Table.Head>
                <Table.Head>Resolution</Table.Head>
                <Table.Head>Frame Rate (fps)</Table.Head>
                <Table.Head>Alerts Generated</Table.Head>
                <Table.Head>False Alerts</Table.Head>
                <Table.Head>Recent User Actions</Table.Head>
              </Table.Row>
            </Table.Header>
            <Table.Body>
              <!-- {#each logs.slice((currentPage - 1) * rowsPerPage, currentPage * rowsPerPage) as log} -->
              {#each Array(50) as _}
                <Table.Row class="border-b-[2px] border-opacity-40 ">
                  <Table.Cell>Camera 123</Table.Cell>
                  <Table.Cell>Locaiton</Table.Cell>
                  <Table.Cell>99.9</Table.Cell>
                  <Table.Cell>0.1</Table.Cell>
                  <Table.Cell>2024-06-01</Table.Cell>
                  <Table.Cell>10</Table.Cell>
                  <Table.Cell>1080p</Table.Cell>
                  <Table.Cell>30</Table.Cell>
                  <Table.Cell>20</Table.Cell>
                  <Table.Cell>2</Table.Cell>
                  <Table.Cell>View, Review</Table.Cell>
                </Table.Row>
              {/each}
              <!-- {/each} -->
            </Table.Body>
          </Table.Root>

          <!-- <div class="flex flex-row items-center justify-center space-x-4 py-4 ">
            <button class="flex flex-row items-center bg-transparent hover:bg-transparent text-white gap-2"
            disabled={currentPage === 1} on:click={() => currentPage--}>
              <ArrowLeft size={18}/>
            Previous</button>
           
            <div class="flex flex-row gap-2 items-center text-sm text-muted-foreground">
              <span class="p-2 rounded-md aspect-square bg-[#084BAE] bg-opacity-20">{currentPage < 10 ? '0' + currentPage : currentPage}</span> of <span class="p-2 rounded-md aspect-square bg-[#084BAE] bg-opacity-20">{Math.ceil(logs.length / rowsPerPage) < 10 ? '0' + Math.ceil(logs.length / rowsPerPage) : Math.ceil(logs.length / rowsPerPage)}</span> Page.
            </div>
            
            <button class="flex flex-row items-center bg-transparent hover:bg-transparent text-white gap-2"
            disabled={currentPage * rowsPerPage >= logs.length} on:click={() => currentPage++}>Next
            <ArrowRight size={18}/></button>
          
          </div> -->
        </div>
      {/if}
      {#if view === 2}
        <div class="flex flex-col gap-4 p-4 mt-8">
          <div class="flex flex-row justify-between items-center">
            <div class="flex items-center gap-5">
              <span
                class="text-[#3c3c3c] font-semibold p-2 rounded-md bg-[#F1f1f1]"
                >Total Nodes: $$</span
              >
              <span class="text-[#6B6B6B] font-semibold">Not Started: $$</span>
              <span class="text-[#E2A925] font-semibold">Faulty $$</span>
              <span class="text-[#55DE64] font-semibold">Running: $$</span>
              <span class="text-[#F93333] font-semibold">Not Working: $$</span>
            </div>
            <div class="flex items-center gap-5">
              <button class="flex items-center gap-2"
                ><Filter size={20} /> Filters</button
              >
              <span class="relative">
                <Input placeholder="Search" class="w-[250px] pl-8" />
                <Search
                  size={18}
                  class="absolute top-1/2 -translate-y-1/2 left-2"
                />
              </span>
            </div>
          </div>
          <Table.Root>
            <Table.Header
              class="bg-[#F9f9f9] rounded-2xl border border-solid border-[#e4e4e4]"
            >
              <Table.Row class="bg-transparent border-b-0 w-full rounded-2xl">
                <Table.Head>Node Name</Table.Head>
                <Table.Head>Location</Table.Head>
                <Table.Head>Uptime (%)</Table.Head>
                <Table.Head>Downtime (%)</Table.Head>
                <Table.Head>Last Maintenance</Table.Head>
                <Table.Head>Storage Capacity (GB)</Table.Head>
                <Table.Head>Available Storage</Table.Head>
                <Table.Head>Connected Cameras</Table.Head>
                <Table.Head>Alerts Generated</Table.Head>
                <Table.Head>False Alerts</Table.Head>
                <Table.Head>Recent User Actions</Table.Head>
              </Table.Row>
            </Table.Header>
            <Table.Body>
              <!-- {#each logs.slice((currentPage - 1) * rowsPerPage, currentPage * rowsPerPage) as log} -->
              {#each Array(50) as _}
                <Table.Row class="border-b-[2px] border-opacity-40 ">
                  <Table.Cell>Node 123</Table.Cell>
                  <Table.Cell>Locaiton</Table.Cell>
                  <Table.Cell>99.9</Table.Cell>
                  <Table.Cell>0.1</Table.Cell>
                  <Table.Cell>2024-06-01</Table.Cell>
                  <Table.Cell>20</Table.Cell>
                  <Table.Cell>5</Table.Cell>
                  <Table.Cell>22</Table.Cell>
                  <Table.Cell>200</Table.Cell>
                  <Table.Cell>12</Table.Cell>
                  <Table.Cell>Login, Configure</Table.Cell>
                </Table.Row>
              {/each}
              <!-- {/each} -->
            </Table.Body>
          </Table.Root>

          <!-- <div class="flex flex-row items-center justify-center space-x-4 py-4 ">
            <button class="flex flex-row items-center bg-transparent hover:bg-transparent text-white gap-2"
            disabled={currentPage === 1} on:click={() => currentPage--}>
              <ArrowLeft size={18}/>
            Previous</button>
           
            <div class="flex flex-row gap-2 items-center text-sm text-muted-foreground">
              <span class="p-2 rounded-md aspect-square bg-[#084BAE] bg-opacity-20">{currentPage < 10 ? '0' + currentPage : currentPage}</span> of <span class="p-2 rounded-md aspect-square bg-[#084BAE] bg-opacity-20">{Math.ceil(logs.length / rowsPerPage) < 10 ? '0' + Math.ceil(logs.length / rowsPerPage) : Math.ceil(logs.length / rowsPerPage)}</span> Page.
            </div>
            
            <button class="flex flex-row items-center bg-transparent hover:bg-transparent text-white gap-2"
            disabled={currentPage * rowsPerPage >= logs.length} on:click={() => currentPage++}>Next
            <ArrowRight size={18}/></button>
          
          </div> -->
        </div>
      {/if}
    </section>
  {/if}
</div>
