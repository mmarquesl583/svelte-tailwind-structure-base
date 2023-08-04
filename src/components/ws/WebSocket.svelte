<script>
  import { onMount } from "svelte"
  import Card from "../card/Card.svelte"
  import mock from "./mock"

  let conds = mock;

  const updateOnline = (data) => {
    var found = false
    for (const i in conds) {
      for (const j in conds[i].devices) {
        if (conds[i].devices[j].ip == data.ip) {
          conds[i].devices[j].online = data.online
          found = true
          break
        }
      }
      if (found) break
    }
    conds = [...conds];
  }


  onMount(() => {
    const ws = new WebSocket("ws://192.168.3.102:8090");
    ws.onmessage = (e) => {
      const msg = JSON.parse(e.data)
      if (msg.event == "status") {
        updateOnline(msg.data.device)
      }
    }
  })
</script>

{#each conds as data}
  <Card {data} />
{/each}