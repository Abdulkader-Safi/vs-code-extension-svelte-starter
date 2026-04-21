<script lang="ts">
  import { vscode } from "../vscodeApi";

  let message = $state("Hello from Svelte!");

  function handleSendNotification() {
    vscode.postMessage({
      type: "showNotification",
      message,
    });
  }
</script>

<div class="max-w-2xl mx-auto">
  <h1 class="mb-6 text-3xl font-bold">Send Notification</h1>

  <div class="p-6 bg-gray-800 rounded-lg shadow-lg">
    <div class="mb-4">
      <label for="message" class="block mb-2 text-sm font-medium">
        Notification Message
      </label>
      <input
        id="message"
        type="text"
        bind:value={message}
        class="w-full px-3 py-2 bg-gray-700 border border-gray-600 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-transparent"
        placeholder="Enter your message..."
      />
    </div>

    <button
      type="button"
      onclick={handleSendNotification}
      class="w-full px-4 py-2 font-medium text-white transition-colors duration-200 bg-blue-600 rounded-md hover:bg-blue-700 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:ring-offset-2 focus:ring-offset-gray-800"
    >
      Send Notification
    </button>

    <div class="p-4 mt-6 bg-gray-700 rounded-md">
      <h3 class="mb-2 text-sm font-medium">How it works:</h3>
      <p class="text-sm text-gray-300">
        This button sends a message to the VSCode extension, which then
        displays a notification using
        <code class="px-1 py-0.5 bg-gray-600 rounded text-xs">
          vscode.window.showInformationMessage()
        </code>
      </p>
    </div>
  </div>
</div>
