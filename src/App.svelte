<script>
  import { Button } from "carbon-components-svelte";
  import { DatePicker, DatePickerInput } from "carbon-components-svelte";
  import { Modal } from "carbon-components-svelte";
  import { DataTable, Link } from "carbon-components-svelte";
  import PlayOutline32 from "carbon-icons-svelte/lib/PlayOutline32";
  import StopOutline32 from "carbon-icons-svelte/lib/StopOutline32";
  import Restart32 from "carbon-icons-svelte/lib/Restart32";

  let open = false;
  let agents = [
    {
      id: "1",
      name: "SW-123-1",
      address: "127.0.0.1",
      port: 161,
      state: "Running",
    },
    {
      id: "2",
      name: "SW-123-2",
      address: "127.0.0.2",
      port: 161,
      state: "Stopped",
    },
  ];
</script>

<main>
  <h1>Client of simulated SNMP agents</h1>

  <DatePicker
    dateFormat="d/m/Y"
    datePickerType="single"
    style="float:right;margin-top:50px;"
  >
    <DatePickerInput
      labelText="Programmer un démarrage des agents"
      placeholder="jj/mm/aaaa"
    />
  </DatePicker>

  <DataTable
    style="margin-top:100px;"
    headers={[
      { key: "name", value: "Name" },
      { key: "address", value: "Address" },
      { key: "port", value: "Port" },
      { key: "state", value: "State" },
      { key: "actions", value: "Actions" },
    ]}
    rows={agents}
  >
    <span slot="cell-header" let:header>
      {#if header.key === "port"}
        {header.value}
        (network)
      {:else}{header.value}{/if}
    </span>
    <span slot="cell" let:row let:cell>
      {#if cell.key === "name"}
        <Link href="#">
          {cell.value}
        </Link>
      {:else if cell.key === "state"}
        {#if cell.value === "Running"}
          <span style="color:green;">
            <strong>{cell.value}</strong>
          </span>
        {:else if cell.value === "Stopped"}
          <span style="color:red;">
            <strong>{cell.value}</strong>
          </span>
        {:else}
          <strong>{cell.value}</strong>
        {/if}
      {:else if cell.key === "actions"}
        <Link href="#" title="Play">
          <PlayOutline32 style="color:#2ecc71;" />
        </Link>
        <Link href="#" title="Stop">
          <StopOutline32 style="color:#d63031;" />
        </Link>
        <Link href="#" title="Restart">
          <Restart32 style="color:#0984e3;" />
        </Link>
      {:else}
        {cell.value}
      {/if}
    </span>
  </DataTable>

  <Button style="margin-top:50px;float:right;" on:click={() => (open = true)}
    >Démarrer</Button
  >
  <Modal
    bind:open
    modalHeading="Êtes vous sûr de vouloir démarrer tous les agents ?"
    secondaryButtonText="Annuler"
    primaryButtonText="Oui"
    on:click:button--secondary={() => (open = false)}
    on:open
    on:close
    on:submit
  >
    <p>Une fois cette action effectuée, tous les agents SNMP démarreront</p>
  </Modal>
</main>

<style>
  main {
    text-align: center;
    padding: 1em;
    max-width: 240px;
    margin: 0 auto;
  }

  h1 {
    color: #1f1f1f;
    text-transform: uppercase;
    font-size: 4em;
    font-weight: 100;
    padding-top: 20px;
  }

  @media (min-width: 640px) {
    main {
      max-width: none;
    }
  }
</style>
