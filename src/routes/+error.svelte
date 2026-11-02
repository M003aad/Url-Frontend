<!-- src/routes/+error.svelte -->
<script lang='ts'>
    import '../app.css';
    import { page } from '$app/state';
</script>

<div class="flex flex-col items-center justify-center min-h-[60vh] text-center p-4">
  {#if page.status === 404}
    <h1 class="text-6xl font-bold text-indigo-600">404</h1>
    <h2 class="text-2xl font-semibold mt-2">Page Not Found</h2>
  {:else}
    <h1 class="text-6xl font-bold text-red-600">{page.status}</h1>
    <h2 class="text-2xl font-semibold mt-2">An error occurred</h2>
    <p class="text-gray-600 mt-1">{page.error?.message || 'Something went wrong.'}</p>
  {/if}

  <a 
    href="/" 
    class="mt-6 px-4 py-2 bg-indigo-600 text-white font-medium rounded-lg hover:bg-indigo-700 transition"
  >
    Back to Home
  </a>
</div>