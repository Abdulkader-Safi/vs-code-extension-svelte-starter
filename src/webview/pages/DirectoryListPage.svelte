<script lang="ts">
  import { onMount, onDestroy } from "svelte";
  import { vscode } from "../vscodeApi";

  interface DirectoryItem {
    name: string;
    type: "file" | "directory";
  }

  interface DirectoryData {
    contents?: DirectoryItem[];
    path?: string;
    error?: string;
  }

  let directoryData = $state<DirectoryData | null>(null);
  let loading = $state(false);

  function handleMessage(event: MessageEvent) {
    const msg = event.data;
    if (msg?.type === "directoryContents") {
      directoryData = msg.data;
      loading = false;
    }
  }

  function handleLoadDirectory() {
    loading = true;
    vscode.postMessage({ type: "getDirectoryContents" });
  }

  onMount(() => {
    window.addEventListener("message", handleMessage);
    handleLoadDirectory();
  });

  onDestroy(() => {
    window.removeEventListener("message", handleMessage);
  });
</script>

<div class="max-w-4xl mx-auto">
  <h1 class="mb-6 text-3xl font-bold">Directory Contents</h1>

  <div class="p-6 bg-gray-800 rounded-lg shadow-lg">
    {#if directoryData}
      {#if directoryData.error}
        <div class="p-4 border border-red-700 rounded-md bg-red-900/30">
          <p class="text-red-400">{directoryData.error}</p>
        </div>
      {:else}
        {#if directoryData.path}
          <div class="p-3 mb-4 bg-gray-700 rounded-md">
            <p class="text-sm text-gray-300">
              <span class="font-medium">Path:</span>
              <code class="text-blue-400">{directoryData.path}</code>
            </p>
          </div>
        {/if}

        {#if directoryData.contents && directoryData.contents.length > 0}
          <div class="space-y-2">
            <h3 class="mb-3 text-lg font-medium">
              Items ({directoryData.contents.length})
            </h3>
            <div class="overflow-hidden bg-gray-700 rounded-md">
              {#each directoryData.contents as item, index (index)}
                <div
                  class="flex items-center px-4 py-3 transition-colors border-b border-gray-600 last:border-b-0 hover:bg-gray-600"
                >
                  <span class="mr-3 text-xl">
                    {item.type === "directory" ? "📁" : "📄"}
                  </span>
                  <span class="flex-1 font-medium">{item.name}</span>
                  <span class="text-xs text-gray-400 uppercase">
                    {item.type}
                  </span>
                </div>
              {/each}
            </div>
          </div>
        {:else}
          <div class="p-4 bg-gray-700 rounded-md">
            <p class="text-gray-400">Directory is empty</p>
          </div>
        {/if}
      {/if}
    {/if}
  </div>
</div>
