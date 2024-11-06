<script lang="ts">
  import AddDevice from "@/components/configuration/AddDevice.svelte";
  import PocketBase from "pocketbase";
  import type { PageServerData } from "./$types";
  import { getContext, onDestroy, onMount } from "svelte";
  import ConfigPanel from "@/components/configuration/ConfigPanel.svelte";
  import type { Node, User, Camera } from "@/types";
  import { selectedNode } from "@/stores";
  import { ChevronLeft, ChevronRight } from "lucide-svelte";
  import Configuration from "@/components/configuration/Configuration.svelte";
  import Subscription from "@/components/configuration/Subscription.svelte";
  import ContactUs from "@/components/configuration/ContactUs.svelte";
  import HelpCenter from "@/components/configuration/HelpCenter.svelte";
  import About from "@/components/configuration/About.svelte";
  import Logout from "@/components/configuration/Logout.svelte";
  import { writable } from "svelte/store";
  const user: User = getContext("user");
//   export let data: PageServerData;
  const session = data?.session;
  let nodes: Node[] = [];

  let activeSec = writable<null | number>(null);
  const PB = new PocketBase(`https://pocketbase.lenscorp.cloud`);

  async function getNodes(): Promise<Node[]> {
    if (session?.node.length > 0) {
      const nodes = await PB.collection("node").getFullList(200, {
        sort: "-created",
        expand: "camera",
        filter: `session~"${session.id}"`,
      });
      return nodes.map(
        (node) =>
          ({
            ...node,
            session: session.id,
            camera:
              node.camera.length > 0
                ? (node.expand.camera.reverse().map((cam: Camera) => ({
                    name: cam.name,
                    id: cam.id,
                    url: cam.url,
                  })) as Camera[])
                : [],
          }) as unknown as Node
      );
    }
    return [];
  }

  const menuList = [
    {
      text: "Add Device",
      icon: "/assets/vms/device-imac-plus1.svg",
      iconS: "/assets/vms/device-imac-plus2.svg",
    },
    {
      text: "Configuration",
      icon: "/assets/vms/configure1.svg",
      iconS: "/assets/vms/configure2.svg",
    },
    {
      text: "Subscription",
      icon: "/assets/vms/subscriptions1.svg",
      iconS: "/assets/vms/subscriptions2.svg",
    },
    {
      text: "Contact Us",
      icon: "/assets/vms/perm-contact-calendar-sharp1.svg",
      iconS: "/assets/vms/perm-contact-calendar-sharp2.svg",
    },
    {
      text: "Help Centre",
      icon: "/assets/vms/help-outline1.svg",
      iconS: "/assets/vms/help-outline2.svg",
    },
    {
      text: "About",
      icon: "/assets/vms/about-circle-outline1.svg",
      iconS: "/assets/vms/about-circle-outline2.svg",
    },
    {
      text: "Log Out",
      icon: "/assets/vms/log-out1.svg",
      iconS: "/assets/vms/log-out2.svg",
    },
  ];

  onMount(async () => {
    nodes = await getNodes();
    selectedNode.set(nodes[0]);
  });
</script>

<!-- desk -->
<main
  class="hidden sm:flex items-start justify-between w-full overflow-y-hidden min-h-[calc(100vh-75px)]"
>
  <ConfigPanel
    user={null}
    records={data?.records}
    logs={data?.logs}
    sessionId={data?.session?.id}
    {data}
  />
</main>

<!-- mob -->

