<script context="module" lang="ts">
  export function getStorage(name: string, keys: any) {
    const kq: any = {};
    const s = localStorage.getItem(name);
    if (s) {
      const j = JSON.parse(s);
      if (isPlainObject(j)) {
        for (const key in j) {
          if (Object.prototype.hasOwnProperty.call(j, key)) {
            if (keys.indexOf(key) > -1) {
              kq[key] = j[key];
            }
          }
        }
      }
    }
    return kq;
  }
</script>

<script lang="ts">
  import { page } from "$app/stores";
  import "../styles/app.scss";
  import { AppBar, AppShell, TabAnchor, TabGroup } from "@skeletonlabs/skeleton";
  import Fa from "svelte-fa";
  import { faCaretDown, faCaretUp, faClock, faCoins, faHouse } from "@fortawesome/free-solid-svg-icons";
  import { initializeStores, Toast } from "@skeletonlabs/skeleton";
  import { isPlainObject } from "lodash";

  initializeStores();
</script>

<Toast />
<AppShell>
  <svelte:fragment slot="header">
    <TabGroup
      justify="justify-center"
      active="variant-filled-primary"
      hover="hover:variant-soft-primary"
      flex="flex-1 lg:flex-none"
      rounded=""
      border=""
      class="w-full bg-surface-100-800-token"
    >
      <TabAnchor href="/" selected={$page.url.pathname === "/"}>
        <svelte:fragment slot="lead"><Fa icon={faHouse} class="mx-auto" /></svelte:fragment>
        <span>Home</span>
      </TabAnchor>
      <TabAnchor href="/speed" selected={$page.url.pathname === "/speed/"}>
        <svelte:fragment slot="lead"><Fa icon={faClock} class="mx-auto" /></svelte:fragment>
        <span>Speed</span>
      </TabAnchor>
      <TabAnchor href="/rss" selected={$page.url.pathname === "/rss/"}>
        <svelte:fragment slot="lead"><Fa icon={faCoins} class="mx-auto" /></svelte:fragment>
        <span>RSS</span>
      </TabAnchor>
    </TabGroup>
  </svelte:fragment>
  <slot />
</AppShell>
