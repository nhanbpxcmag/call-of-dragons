<script lang="ts">
  import { browser } from "$app/environment";
  import { RadioGroup, RadioItem, getToastStore } from "@skeletonlabs/skeleton";
  import type { ToastSettings, ToastStore } from "@skeletonlabs/skeleton";
  import { keys } from "lodash";
  import { getStorage } from "../+layout.svelte";
  const toastStore = getToastStore();
  let list = [
    {
      title: "1m",
      time: 60,
    },
    {
      title: "5m",
      time: 60 * 5,
    },
    {
      title: "10m",
      time: 60 * 10,
    },
    {
      title: "15m",
      time: 60 * 15,
    },
    {
      title: "30m",
      time: 60 * 30,
    },
    {
      title: "1h",
      time: 60 * 60,
    },
    {
      title: "3h",
      time: 60 * 60 * 3,
    },
    {
      title: "8h",
      time: 60 * 60 * 8,
    },
    {
      title: "15h",
      time: 60 * 60 * 15,
    },
  ];
  let keys_list = keys(list);
  let list_Universal = [
    {
      title: "24h",
      time: 60 * 60 * 24,
    },
    {
      title: "3d",
      time: 60 * 60 * 24 * 3,
    },
    {
      title: "7d",
      time: 60 * 60 * 24 * 7,
    },
    {
      title: "30d",
      time: 60 * 60 * 24 * 30,
    },
  ];
  let keys_list_Universal = keys(list_Universal);
  let value_building: any = {},
    value_Training: any = {},
    value_Research: any = {},
    value_Universal: any = {},
    value_Universal_more: any = {};
  let total_building = 0,
    total_Training = 0,
    total_Research = 0,
    total_Universal = 0;
  let second_training: any,
    lc_training: any,
    calc_kq = { sl: 0, lc: 0 },
    lay_Universal = 1;
  $: {
    if (browser) {
      value_building = getStorage("value_building", keys_list);
      value_Training = getStorage("value_Training", keys_list);
      value_Research = getStorage("value_Research", keys_list);
      value_Universal = getStorage("value_Universal", keys_list);
      value_Universal_more = getStorage("value_Universal_more", keys_list_Universal);
    }
  }
  $: {
    total_building = 0;
    localStorage.setItem("value_building", JSON.stringify(value_building));
    for (const key in value_building) {
      if (Object.prototype.hasOwnProperty.call(value_building, key)) {
        const v = value_building[key];
        const k = key as unknown as number;
        if (v) {
          total_building += list[k].time * v;
        }
      }
    }
  }
  $: {
    total_Training = 0;
    localStorage.setItem("value_Training", JSON.stringify(value_Training));
    for (const key in value_Training) {
      if (Object.prototype.hasOwnProperty.call(value_Training, key)) {
        const v = value_Training[key];
        const k = key as unknown as number;
        if (v) {
          total_Training += list[k].time * v;
        }
      }
    }
  }
  $: {
    total_Research = 0;
    localStorage.setItem("value_Research", JSON.stringify(value_Research));
    for (const key in value_Research) {
      if (Object.prototype.hasOwnProperty.call(value_Research, key)) {
        const v = value_Research[key];
        const k = key as unknown as number;
        if (v) {
          total_Research += list[k].time * v;
        }
      }
    }
  }
  $: {
    total_Universal = 0;
    localStorage.setItem("value_Universal", JSON.stringify(value_Universal));
    for (const key in value_Universal) {
      if (Object.prototype.hasOwnProperty.call(value_Universal, key)) {
        const v = value_Universal[key];
        const k = key as unknown as number;
        if (v) {
          total_Universal += list[k].time * v;
        }
      }
    }
    localStorage.setItem("value_Universal_more", JSON.stringify(value_Universal_more));
    for (const key in value_Universal_more) {
      if (Object.prototype.hasOwnProperty.call(value_Universal_more, key)) {
        const v = value_Universal_more[key];
        const k = key as unknown as number;
        if (v) {
          total_Universal += list_Universal[k].time * v;
        }
      }
    }
  }
  function secondToHHMM(totalSeconds: number) {
    var hours: any = Math.floor(totalSeconds / 3600);
    var minutes: any = Math.floor((totalSeconds - hours * 3600) / 60);
    var seconds: any = totalSeconds - hours * 3600 - minutes * 60;

    return hours + "h " + minutes + "m";
  }
  function secondToDDHHMM(totalSeconds: number) {
    var fm = [
      Math.floor(totalSeconds / 60 / 60 / 24), // DAYS
      Math.floor(totalSeconds / 60 / 60) % 24, // HOURS
      Math.floor(totalSeconds / 60) % 60, // MINUTES
      totalSeconds % 60, // SECONDS
    ];

    return `${fm[0]}d ${fm[1]}h ${fm[2]}min`;
  }
  function calc_training() {
    calc_kq = { sl: 0, lc: 0 };
    if (!second_training) {
      const t: ToastSettings = {
        message: "Vui lòng nhập (Thời gian mỗi lính)",
      };
      toastStore.trigger(t);
      return;
    }
    let total = lay_Universal ? total_Training + total_Universal : total_Training;
    let sl = Math.floor(total / second_training);
    calc_kq = { sl, lc: sl * (lc_training || 0) };
  }
</script>

<div class="container p-8 mx-auto">
  <div class="grid grid-cols-1 gap-x-2 gap-y-2 FYvucEF md:grid-cols-2 xl:grid-cols-4">
    <div>
      <div class="mb-2">Building</div>
      {#each list as { title }, key}
        <div class="input-group input-group-divider grid-cols-[auto_1fr_auto] NciCqJg">
          <div class="w-16 input-group-shim">{title}</div>
          <input
            class="input"
            {title}
            type="number"
            pattern="[0-9]*"
            placeholder="0"
            bind:value={value_building[key]}
          />
        </div>
      {/each}
      <div class="mt-2">
        <div>
          <div class="font-bold text-primary-500">Tổng:</div>
          <div>- {Math.floor(total_building / 60)} min</div>
          <!-- <div>- {secondToHHMM(total_building)}</div> -->
          <div>- {secondToDDHHMM(total_building)}</div>
          <div class="font-bold text-primary-500">Tổng (Universal):</div>
          <div>- {Math.floor((total_building + total_Universal) / 60)} min</div>
          <!-- <div>- {secondToHHMM(total_building + total_Universal)}</div> -->
          <div>- {secondToDDHHMM(total_building + total_Universal)}</div>
        </div>
      </div>
    </div>
    <div>
      <div class="mb-2">Training</div>
      {#each list as { title }, key}
        <div class="input-group input-group-divider grid-cols-[auto_1fr_auto] NciCqJg">
          <div class="w-16 input-group-shim">{title}</div>
          <input
            class="input"
            {title}
            type="number"
            pattern="[0-9]*"
            placeholder="0"
            bind:value={value_Training[key]}
          />
        </div>
      {/each}
      <div class="mt-2">
        <div>
          <div class="font-bold text-primary-500">Tổng:</div>
          <div>- {Math.floor(total_Training / 60)} min</div>
          <!-- <div>- {secondToHHMM(total_Training)}</div> -->
          <div>- {secondToDDHHMM(total_Training)}</div>
          <div class="font-bold text-primary-500">Tổng (Universal):</div>
          <div>- {Math.floor((total_Training + total_Universal) / 60)} min</div>
          <!-- <div>- {secondToHHMM(total_Training + total_Universal)}</div> -->
          <div>- {secondToDDHHMM(total_Training + total_Universal)}</div>
        </div>
      </div>
    </div>
    <div>
      <div class="mb-2">Research</div>
      {#each list as { title }, key}
        <div class="input-group input-group-divider grid-cols-[auto_1fr_auto] NciCqJg">
          <div class="w-16 input-group-shim">{title}</div>
          <input
            class="input"
            {title}
            type="number"
            pattern="[0-9]*"
            placeholder="0"
            bind:value={value_Research[key]}
          />
        </div>
      {/each}
      <div class="mt-2">
        <div>
          <div class="font-bold text-primary-500">Tổng:</div>
          <div>- {Math.floor(total_Research / 60)} min</div>
          <!-- <div>- {secondToHHMM(total_Research)}</div> -->
          <div>- {secondToDDHHMM(total_Research)}</div>
          <div class="font-bold text-primary-500">Tổng (Universal):</div>
          <div>- {Math.floor((total_Research + total_Universal) / 60)} min</div>
          <!-- <div>- {secondToHHMM(total_Research + total_Universal)}</div> -->
          <div>- {secondToDDHHMM(total_Research + total_Universal)}</div>
        </div>
      </div>
    </div>
    <div>
      <div class="mb-2">Universal</div>
      {#each list as { title }, key}
        <div class="input-group input-group-divider grid-cols-[auto_1fr_auto] NciCqJg">
          <div class="w-16 input-group-shim">{title}</div>
          <input
            class="input"
            {title}
            type="number"
            pattern="[0-9]*"
            placeholder="0"
            bind:value={value_Universal[key]}
          />
        </div>
      {/each}
      {#each list_Universal as { title }, key}
        <div class="input-group input-group-divider grid-cols-[auto_1fr_auto] NciCqJg">
          <div class="w-16 input-group-shim">{title}</div>
          <input
            class="input"
            {title}
            type="number"
            pattern="[0-9]*"
            placeholder="0"
            bind:value={value_Universal_more[key]}
          />
        </div>
      {/each}
      <div class="mt-2">
        <div>
          <div class="font-bold text-primary-500">Tổng:</div>
          <div>- {Math.floor(total_Universal / 60)} min</div>
          <!-- <div>- {secondToHHMM(total_Universal)}</div> -->
          <div>- {secondToDDHHMM(total_Universal)}</div>
        </div>
      </div>
    </div>
  </div>
  <div class="flex flex-col justify-center mt-8">
    <div class="h2">Tính train lính</div>
    <div class="mt-5">
      <label class="w-96 label">
        <input
          class="input variant-form-material"
          type="number"
          placeholder="Thời gian mỗi lính"
          bind:value={second_training}
        />
      </label>
      <label class="my-5 w-96 label">
        <input
          class="input variant-form-material"
          type="number"
          placeholder="Lực chiến mỗi lính"
          bind:value={lc_training}
        />
      </label>

      Universal
      <div>
        <RadioGroup active="variant-filled-primary" hover="hover:variant-soft-primary">
          <RadioItem bind:group={lay_Universal} name="justify" value={1}>Có</RadioItem>
          <RadioItem bind:group={lay_Universal} name="justify" value={0}>Không</RadioItem>
        </RadioGroup>
      </div>

      <button type="button" class="mt-5 btn variant-filled-primary" on:click={calc_training}>Tính</button>
    </div>
    <div>Kết quả:</div>
    <div>- Số lượng: {calc_kq.sl}</div>
    <div>- Lực chiến: {calc_kq.lc}</div>
  </div>
</div>

<style>
  .FYvucEF > div {
    @apply bg-surface-700 rounded px-4 py-10;
  }
</style>
