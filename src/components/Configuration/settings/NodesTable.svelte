<script lang="ts">
  import Button from "@/components/ui/button/button.svelte";
  import Checkbox from "@/components/ui/checkbox/checkbox.svelte";
  import * as Table from "@/components/ui/table";
  export let allUsers;
  export let data;

  let userNodeSelections = [];

  function handleSave() {
    userNodeSelections = allUsers.map((user) => {
      return {
        sessionId: user.expand.session[0].id,
        nodeId: data.nodesList
          .filter((node) => user.expand.session[0].node.includes(node.id))
          .map((node) => node.id),
      };
    });

    fetch("/api/node/permissionNode", {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify(userNodeSelections),
    })
      .then((response) => response.json())
      .then((data) => {
        console.log("Success:", data);
      })
      .catch((error) => {
        console.error("Error:", error);
      });
  }
</script>

<Table.Root class="mx-auto w-full flex flex-col">
  <Table.Header
    class="border-2 border-[#e4e4e4] w-max border-solid rounded-lg bg-[#f9f9f9] "
  >
    <Table.Row class="bg-transparent flex items-center justify-between p-3">
      <Table.Head class="text-[#727272] h-full w-[200px]">User/Nodes</Table.Head
      >
      {#if data?.nodesList?.length>0}
      {#each data?.nodesList as node}
        <Table.Head class="text-[#727272] h-full w-[200px]">
          {node.name}
        </Table.Head>
      {/each}
      {/if}
    </Table.Row>
  </Table.Header>
  <Table.Body class="pb-10">
    {#if allUsers}
      {#each allUsers as user}
        <Table.Row
          class="bg-transparent cursor-pointer w-max flex items-center justify-between mt-4 px-3 rounded-lg border-2 border-solid border-[#e4e4e4]"
        >
          <Table.Cell class="text-black h-full w-[200px]">
            <span class="flex flex-col font-semibold text-primary">
              <span>
                Name: {user?.name.length > 0 ? user.name : "-"}
              </span>
              <span class="text-xs">
                Id: {user.id}
              </span>
            </span>
          </Table.Cell>
          {#each data?.nodesList as node}
            <Table.Cell class="text-[#727272] h-full text-sm w-[200px]">
              <!-- <input type="checkbox" checked={user.expand.session[0].node.includes(node.id)}  on:change={(e) => {
                    console.log(e.target.checked)
                    console.log(node.id, user.session[0])

                }}/> -->

              <Checkbox
                class="data-[state=checked]:bg-orange-500 data-[state=checked]:border-orange-500"
                checked={user.expand.session[0].node.includes(node.id)}
                on:change={(e) => {
                  const isChecked = e.target.checked;
                  if (isChecked) {
                    user.expand.session[0].node.push(node.id);
                  } else {
                    const index = user.expand.session[0].node.indexOf(node.id);
                    if (index > -1) {
                      user.expand.session[0].node.splice(index, 1);
                    }
                  }
                }}
              />
            </Table.Cell>
          {/each}
        </Table.Row>
      {/each}
    {/if}
  </Table.Body>
</Table.Root>
<Button class="mr-auto flex" on:click={handleSave}>Save</Button>
