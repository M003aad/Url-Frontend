<!-- src/routes/+layout.svelte -->
<script lang="ts">
	import '../app.css';
	import { page } from '$app/state';

	// Default session state (can be linked to an Auth Store)
	let user = $state<{ firstName: string; email: string } | null>(null);
	let isMobileMenuOpen = $state(false);

	async function handleLogout() {
		try {
			await fetch('http://localhost:5000/auth/logout', {
				method: 'POST',
				credentials: 'include'
			});
			user = null;
			window.location.href = '/login';
		} catch (err) {
			console.error('Logout failed:', err);
		}
	}

	let { children } = $props();
</script>

<div class="min-h-screen flex flex-col bg-slate-50 text-slate-800 font-sans">
	<!-- Navbar Header -->
	<header class="sticky top-0 z-50 bg-white/80 backdrop-blur-md border-b border-slate-200">
		<div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
			<div class="flex justify-between h-16 items-center">
				<!-- Logo -->
				<a
					href="/"
					class="flex items-center gap-2 font-bold text-xl text-indigo-600 hover:text-indigo-700 transition"
				>
					<!-- <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13.828 10.172a4 4 0 00-5.656 0l-4 4a4 4 0 105.656 5.656l1.102-1.101m-.758-4.899a4 4 0 005.656 0l4-4a4 4 0 00-5.656-5.656l-1.1 1.1" />
          </svg> -->
					<span>ShortURL</span>
				</a>

				<!-- Desktop Navigation Links -->
				<nav class="hidden md:flex items-center gap-6 text-sm font-medium">
					<a
						href="/"
						class="hover:text-indigo-600 transition {page.url.pathname === '/'
							? 'text-indigo-600 font-semibold'
							: 'text-slate-600'}"
					>
						Home
					</a>

					{#if user}
						<a
							href="/dashboard"
							class="hover:text-indigo-600 transition {page.url.pathname === '/dashboard'
								? 'text-indigo-600 font-semibold'
								: 'text-slate-600'}"
						>
							Dashboard
						</a>
						<a
							href="/profile"
							class="hover:text-indigo-600 transition {page.url.pathname === '/profile'
								? 'text-indigo-600 font-semibold'
								: 'text-slate-600'}"
						>
							Profile
						</a>
					{/if}
				</nav>

				<!-- Right Side Actions -->
				<div class="hidden md:flex items-center gap-3">
					{#if user}
						<span class="text-sm font-medium text-slate-700 bg-slate-100 px-3 py-1.5 rounded-full">
							Welcome, {user.firstName}
						</span>
						<button
							onclick={handleLogout}
							class="text-sm px-4 py-2 text-red-600 hover:bg-red-50 font-medium rounded-lg transition cursor-pointer"
						>
							Logout
						</button>
					{:else}
						<a
							href="/login"
							class="text-sm px-4 py-2 text-slate-700 hover:text-indigo-600 font-medium transition"
						>
							Sign In
						</a>
						<a
							href="/register"
							class="text-sm px-4 py-2 bg-indigo-600 hover:bg-indigo-700 text-white font-medium rounded-lg transition shadow-sm"
						>
							Get Started
						</a>
					{/if}
				</div>

				<!-- Mobile Menu Toggle Button -->
				<button
					onclick={() => (isMobileMenuOpen = !isMobileMenuOpen)}
					class="md:hidden p-2 rounded-lg text-slate-600 hover:bg-slate-100 focus:outline-none"
					aria-label="Toggle menu"
				>
					<svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
						{#if isMobileMenuOpen}
							<path
								stroke-linecap="round"
								stroke-linejoin="round"
								stroke-width="2"
								d="M6 18L18 6M6 6l12 12"
							/>
						{:else}
							<path
								stroke-linecap="round"
								stroke-linejoin="round"
								stroke-width="2"
								d="M4 6h16M4 12h16M4 18h16"
							/>
						{/if}
					</svg>
				</button>
			</div>
		</div>

		<!-- Mobile Dropdown Menu -->
		{#if isMobileMenuOpen}
			<div class="md:hidden border-b border-slate-200 bg-white px-4 pt-2 pb-4 space-y-2">
				<a
					href="/"
					class="block px-3 py-2 rounded-md text-base font-medium text-slate-700 hover:bg-slate-50"
				>
					Home
				</a>
				{#if user}
					<a
						href="/dashboard"
						class="block px-3 py-2 rounded-md text-base font-medium text-slate-700 hover:bg-slate-50"
					>
						Dashboard
					</a>
					<a
						href="/profile"
						class="block px-3 py-2 rounded-md text-base font-medium text-slate-700 hover:bg-slate-50"
					>
						Profile
					</a>
					<button
						onclick={handleLogout}
						class="w-full text-left block px-3 py-2 rounded-md text-base font-medium text-red-600 hover:bg-red-50"
					>
						Logout
					</button>
				{:else}
					<a
						href="/login"
						class="block px-3 py-2 rounded-md text-base font-medium text-slate-700 hover:bg-slate-50"
					>
						Sign In
					</a>
					<a
						href="/register"
						class="block px-3 py-2 rounded-md text-base font-medium text-indigo-600 hover:bg-indigo-50 font-semibold"
					>
						Get Started
					</a>
				{/if}
			</div>
		{/if}
	</header>

	<!-- Page Content Container -->
	<main class="flex-1 max-w-7xl w-full mx-auto px-4 sm:px-6 lg:px-8 py-8">
		{@render children()}
	</main>

	<!-- Footer -->
	<footer class="bg-white border-t border-slate-200 py-6 text-center text-sm text-slate-500">
		<p>© {new Date().getFullYear()} ShortURL. All rights reserved.</p>
	</footer>
</div>
