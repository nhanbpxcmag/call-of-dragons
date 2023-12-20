<script lang="ts">
  import { browser } from "$app/environment";
  import { RadioGroup, RadioItem, getToastStore } from "@skeletonlabs/skeleton";
  import type { ToastSettings, ToastStore } from "@skeletonlabs/skeleton";
  const toastStore = getToastStore();
  let list = [
    {
      title: "1.000",
      time: 1000,
    },
    {
      title: "10.000",
      time: 1000 * 10,
    },
    {
      title: "50.000",
      time: 1000 * 50,
    },
    {
      title: "150.000",
      time: 1000 * 150,
    },
    {
      title: "500.000",
      time: 1000 * 500,
    },
    {
      title: "1.500.000",
      time: 1000 * 1500,
    },
    {
      title: "5.000.000",
      time: 1000 * 5000,
    },
  ];
  let list_quang = [
    {
      title: "750",
      time: 750,
    },
    {
      title: "7.500",
      time: 7500,
    },
    {
      title: "37.500",
      time: 37500,
    },
    {
      title: "112.500",
      time: 112500,
    },
    {
      title: "375.000",
      time: 375000,
    },
    {
      title: "1.125.000",
      time: 1125000,
    },
    {
      title: "3.750.000 ",
      time: 3750000,
    },
  ];
  let list_nuoc = [
    {
      title: "500",
      time: 500,
    },
    {
      title: "3.000",
      time: 3000,
    },
    {
      title: "15.000",
      time: 15000,
    },
    {
      title: "50.000",
      time: 50000,
    },
    {
      title: "200.000",
      time: 200000,
    },
    {
      title: "600.000",
      time: 600000,
    },
    {
      title: "2.000.000 ",
      time: 2000000,
    },
  ];
  let value_vang: any = {},
    value_go: any = {},
    value_quang: any = {},
    value_nuoc: any = {};
  let value_vang_tp = 0,
    value_go_tp = 0,
    value_quang_tp = 0,
    value_nuoc_tp = 0;
  let total_vang = 0,
    total_go = 0,
    total_quang = 0,
    total_nuoc = 0;
  $: {
    if (browser) {
      if (localStorage.getItem("value_vang_rss")) {
        value_vang = JSON.parse(localStorage.getItem("value_vang_rss") || "");
      }
      if (localStorage.getItem("value_go_rss")) {
        value_go = JSON.parse(localStorage.getItem("value_go_rss") || "");
      }
      if (localStorage.getItem("value_quang_rss")) {
        value_quang = JSON.parse(localStorage.getItem("value_quang_rss") || "");
      }
      if (localStorage.getItem("value_nuoc_rss")) {
        value_nuoc = JSON.parse(localStorage.getItem("value_nuoc_rss") || "");
      }
    }
  }
  $: {
    total_vang = 0;
    localStorage.setItem("value_vang_rss", JSON.stringify(value_vang));
    for (const key in value_vang) {
      if (Object.prototype.hasOwnProperty.call(value_vang, key)) {
        const v = value_vang[key];
        const k = key as unknown as number;
        if (v) {
          total_vang += list[k].time * v;
        }
      }
    }
    total_vang += value_vang_tp || 0;
  }
  $: {
    total_go = 0;
    localStorage.setItem("value_go_rss", JSON.stringify(value_go));
    for (const key in value_go) {
      if (Object.prototype.hasOwnProperty.call(value_go, key)) {
        const v = value_go[key];
        const k = key as unknown as number;
        if (v) {
          total_go += list[k].time * v;
        }
      }
    }

    total_go += value_go_tp || 0;
  }
  $: {
    total_quang = 0;
    localStorage.setItem("value_quang_rss", JSON.stringify(value_quang));
    for (const key in value_quang) {
      if (Object.prototype.hasOwnProperty.call(value_quang, key)) {
        const v = value_quang[key];
        const k = key as unknown as number;
        if (v) {
          total_quang += list_quang[k].time * v;
        }
      }
    }
    total_quang += value_quang_tp || 0;
  }
  $: {
    total_nuoc = 0;
    localStorage.setItem("value_nuoc_rss", JSON.stringify(value_nuoc));
    for (const key in value_nuoc) {
      if (Object.prototype.hasOwnProperty.call(value_nuoc, key)) {
        const v = value_nuoc[key];
        const k = key as unknown as number;
        if (v) {
          total_nuoc += list_nuoc[k].time * v;
        }
      }
    }
    total_nuoc += value_nuoc_tp || 0;
  }
  function format_number(num: number) {
    return new Intl.NumberFormat().format(num).replace(/[,]/g, ".");
  }
</script>

<div class="container p-8 mx-auto">
  <div class="grid grid-cols-1 gap-x-2 gap-y-2 FYvucEF md:grid-cols-2 xl:grid-cols-4">
    <div>
      <div class="mb-2">Vàng {format_number(total_vang)}</div>
      {#each list as { title }, key}
        <div class="input-group input-group-divider grid-cols-[auto_1fr_auto] NciCqJg">
          <div class="w-24 input-group-shim">{title}</div>
          <input class="input" {title} type="number" placeholder="0" bind:value={value_vang[key]} />
        </div>
      {/each}
      <div class="input-group input-group-divider grid-cols-[auto_1fr_auto] NciCqJg">
        <div class="w-24 input-group-shim">Trong TP</div>
        <input class="input" type="number" placeholder="0" bind:value={value_vang_tp} />
      </div>
      <div class="mt-2">
        <div>
          <div>Tổng: {format_number(total_vang)}</div>
        </div>
      </div>
    </div>
    <div>
      <div class="mb-2">Gỗ {format_number(total_go)}</div>
      {#each list as { title }, key}
        <div class="input-group input-group-divider grid-cols-[auto_1fr_auto] NciCqJg">
          <div class="w-24 input-group-shim">{title}</div>
          <input class="input" {title} type="number" placeholder="0" bind:value={value_go[key]} />
        </div>
      {/each}
      <div class="input-group input-group-divider grid-cols-[auto_1fr_auto] NciCqJg">
        <div class="w-24 input-group-shim">Trong TP</div>
        <input class="input" type="number" placeholder="0" bind:value={value_go_tp} />
      </div>
      <div class="mt-2">
        <div>
          <div>Tổng: {format_number(total_go)}</div>
        </div>
      </div>
    </div>
    <div>
      <div class="mb-2">Quặng {format_number(total_quang)}</div>
      {#each list_quang as { title }, key}
        <div class="input-group input-group-divider grid-cols-[auto_1fr_auto] NciCqJg">
          <div class="w-24 input-group-shim">{title}</div>
          <input class="input" {title} type="number" placeholder="0" bind:value={value_quang[key]} />
        </div>
      {/each}
      <div class="input-group input-group-divider grid-cols-[auto_1fr_auto] NciCqJg">
        <div class="w-24 input-group-shim">Trong TP</div>
        <input class="input" type="number" placeholder="0" bind:value={value_quang_tp} />
      </div>
      <div class="mt-2">
        <div>
          <div>Tổng: {format_number(total_quang)}</div>
        </div>
      </div>
    </div>
    <div>
      <div class="mb-2">Nước {format_number(total_nuoc)}</div>
      {#each list_nuoc as { title }, key}
        <div class="input-group input-group-divider grid-cols-[auto_1fr_auto] NciCqJg">
          <div class="w-24 input-group-shim">{title}</div>
          <input class="input" {title} type="number" placeholder="0" bind:value={value_nuoc[key]} />
        </div>
      {/each}
      <div class="input-group input-group-divider grid-cols-[auto_1fr_auto] NciCqJg">
        <div class="w-24 input-group-shim">Trong TP</div>
        <input class="input" type="number" placeholder="0" bind:value={value_nuoc_tp} />
      </div>
      <div class="mt-2">
        <div>
          <div>Tổng: {format_number(total_nuoc)}</div>
        </div>
      </div>
    </div>
  </div>
</div>

<style>
  .FYvucEF > div {
    @apply bg-surface-700 rounded px-4 py-10;
  }
</style>
