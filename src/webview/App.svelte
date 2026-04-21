<script lang="ts">
  import Router, { link, location as currentRoute } from "svelte-spa-router";
  import NotificationPage from "./pages/NotificationPage.svelte";
  import DirectoryListPage from "./pages/DirectoryListPage.svelte";

  const routes = {
    "/": NotificationPage,
    "/directory": DirectoryListPage,
  };
</script>

<div class="min-h-screen text-gray-100 bg-gray-900">
  <nav class="bg-gray-800 border-b border-gray-700">
    <div class="px-4 py-3">
      <div class="flex space-x-4">
        <a
          use:link
          href="/"
          class="px-3 py-2 text-sm font-medium transition-colors rounded-md hover:bg-gray-700 {$currentRoute === '/' ? 'bg-gray-700' : ''}"
        >
          Notification
        </a>
        <a
          use:link
          href="/directory"
          class="px-3 py-2 text-sm font-medium transition-colors rounded-md hover:bg-gray-700 {$currentRoute === '/directory' ? 'bg-gray-700' : ''}"
        >
          Directory
        </a>
      </div>
    </div>
  </nav>

  <main class="container px-4 py-8 mx-auto">
    <Router {routes} />
  </main>
</div>
