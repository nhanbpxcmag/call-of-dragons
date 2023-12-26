<script lang="ts">
  import { browser } from "$app/environment";
  import { debounce, isNumber, toNumber } from "lodash";
  import Layout, { getStorage } from "../+layout.svelte";

  let ngay: any,
    gio: any,
    phut: any,
    giay: any,
    buff_san: any = 39.8406,
    buff_tru: any,
    help: any,
    tg_truoc_giam = "",
    kq = "",
    tg_help = "",
    kq_sau_help = "";
  let key_storage = {
    buff_san: "calculator_buff_san",
    buff_tru: "calculator_buff_tru",
    help: "calculator_help",
    time: "calculator_time",
  };
  function reset() {
    ngay = 0;
    gio = 0;
    phut = 0;
    giay = 0;
  }
  function to_number(value: any): number {
    const num = toNumber(value);
    return isNaN(num) ? 0 : num;
  }

  $: {
    if (browser) {
      buff_san = localStorage.getItem(key_storage.buff_san) || 39.8406;
      buff_tru = localStorage.getItem(key_storage.buff_tru);
      help = localStorage.getItem(key_storage.help);
      let temp = getStorage(key_storage.time, ["ngay", "gio", "phut", "giay"]);
      ngay = temp?.ngay;
      gio = temp?.gio;
      phut = temp?.phut;
      giay = temp?.giay;
    }
  }

  $: {
    localStorage.setItem(key_storage.buff_san, buff_san);
    localStorage.setItem(key_storage.buff_tru, buff_tru);
    localStorage.setItem(key_storage.help, help);
    let temp = { ngay, gio, phut, giay };
    localStorage.setItem(key_storage.time, JSON.stringify(temp));
  }

  function secondToDDHHMMSS(totalSeconds: number) {
    var fm = [
      Math.floor(totalSeconds / 60 / 60 / 24), // DAYS
      (Math.floor(totalSeconds / 60 / 60) % 24).toString().padStart(2, "0"), // HOURS
      (Math.floor(totalSeconds / 60) % 60).toString().padStart(2, "0"), // MINUTES
      (totalSeconds % 60).toString().padStart(2, "0"), // SECONDS
    ];

    return `${fm[0]} Ngày, ${fm[1]}:${fm[2]}:${fm[3]}`;
  }

  function calculator() {
    kq = "";
    tg_truoc_giam = "";
    tg_help = "";
    kq_sau_help = "";
    let time = {
      ngay: to_number(ngay),
      gio: to_number(gio),
      phut: to_number(phut),
      giay: to_number(giay),
    };
    let tong_seconds_help = 0;
    let tong_seconds_sau_help = 0;
    let num_buff_san = to_number(buff_san);
    let num_buff_tru = to_number(buff_tru);
    let num_help = to_number(help);
    let tyle_san = 100 - num_buff_san;
    let tyle_tru = 100 - num_buff_san - num_buff_tru;
    let to_seconds = time.ngay * 24 * 60 * 60 + time.gio * 60 * 60 + time.phut * 60 + time.giay;
    let tong_seconds_da_tru = Math.round((to_seconds * 100) / tyle_san); //thời gian không tính bất kỳ buff nào
    let _phantram = Math.round(((tong_seconds_da_tru - to_seconds) * num_buff_tru) / 100);
    let kq_seconds = Math.round(to_seconds - _phantram); // thời gian tính buff sẵn + buff đá
    tg_truoc_giam = secondToDDHHMMSS(to_seconds);
    kq = secondToDDHHMMSS(kq_seconds);
    if (num_help > 0) {
      let temp = kq_seconds;
      let tong_seconds_help = 0;
      // let _1_lan_help = kq_seconds / 100;
      // if (_1_lan_help < 60) _1_lan_help = 60;
      // tong_seconds_help = _1_lan_help * num_help > kq_seconds ? kq_seconds : _1_lan_help * num_help;
      while (num_help > 0) {
        let _1_lan_help = temp / 100;
        if (_1_lan_help < 60) _1_lan_help = 60;
        temp = temp - _1_lan_help;
        tong_seconds_help += _1_lan_help;
        num_help--;
      }
      tg_help = secondToDDHHMMSS(Math.round(tong_seconds_help));
      kq_sau_help = secondToDDHHMMSS(Math.round(kq_seconds - tong_seconds_help));
    }
  }
</script>

<svelte:document
  on:keyup={(e) => {
    if (e.key === "Enter") calculator();
    // console.log(e);
  }}
/>
<div class="container p-8 mx-auto">
  <div class="flex flex-col items-center justify-center">
    <div class="w-72">
      <div class="mb-8">
        <button type="button" class="btn variant-filled-secondary" on:click={reset}>Reset</button>
      </div>
      <div class="input-group input-group-divider grid-cols-[auto_70px] NciCqJg">
        <input class="input" title="Ngày" type="number" pattern="[0-9]*" placeholder="0" bind:value={ngay} />
        <div class="input-group-shim">Ngày</div>
      </div>
      <div class="input-group input-group-divider grid-cols-[auto_70px] NciCqJg">
        <input class="input" title="Giờ" type="number" pattern="[0-9]*" placeholder="0" bind:value={gio} />
        <div class="input-group-shim">Giờ</div>
      </div>
      <div class="input-group input-group-divider grid-cols-[auto_70px] NciCqJg">
        <input class="input" title="Phút" type="number" pattern="[0-9]*" placeholder="0" bind:value={phut} />
        <div class="input-group-shim">Phút</div>
      </div>
      <div class="input-group input-group-divider grid-cols-[auto_70px] NciCqJg">
        <input class="input" title="Giây" type="number" pattern="[0-9]*" placeholder="0" bind:value={giay} />
        <div class="input-group-shim">Giây</div>
      </div>
      <div class="mt-8 h3">
        <p>Buff</p>
      </div>
      <div>
        <div class="input-group input-group-divider grid-cols-[80px_auto_70px] NciCqJg">
          <div class="input-group-shim">sẵn có</div>
          <input class="input" title="Giây" type="number" pattern="[0-9]*" placeholder="0" bind:value={buff_san} />
          <div class="flex !justify-center input-group-shim">%</div>
        </div>
      </div>
      <div class="mt-2">
        <div class="input-group input-group-divider grid-cols-[80px_auto_70px] NciCqJg">
          <div class="input-group-shim">sẽ trừ</div>
          <input class="input" title="Giây" type="number" pattern="[0-9]*" placeholder="0" bind:value={buff_tru} />
          <div class="flex !justify-center input-group-shim">%</div>
        </div>
      </div>
      <div class="mt-8">
        <div class="input-group input-group-divider grid-cols-[80px_auto_70px] NciCqJg">
          <div class="input-group-shim">Help</div>
          <input class="input" type="number" pattern="[0-9]*" placeholder="0" bind:value={help} />
          <div class="flex !justify-center input-group-shim">lần</div>
        </div>
      </div>
      <div class="mt-8">
        <button type="button" class="mt-5 btn variant-filled-primary" on:click={calculator}>Tính</button>
      </div>
    </div>
    {#if kq}
      <div class="my-5 h2">Kết quả:</div>
      <div class="grid grid-cols-2 gap-x-2">
        <div>Thời gian chưa giảm:</div>
        <div class="text-primary-500">{tg_truoc_giam}</div>
        <div>Thời gian sau giảm:</div>
        <div class="text-primary-500">{kq}</div>
        {#if tg_help}
          <div>Thời gian help:</div>
          <div class="text-primary-500">{tg_help}</div>
        {/if}
        {#if kq_sau_help}
          <div>Thời gian sau help:</div>
          <div class="text-primary-500">{kq_sau_help}</div>
        {/if}
      </div>
    {/if}
  </div>
</div>
