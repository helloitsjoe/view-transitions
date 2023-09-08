<script>
	import { page } from '$app/stores';
	import { onNavigate } from '$app/navigation';

	// https://svelte.dev/blog/view-transitions
	onNavigate((navigation) => {
		if (!document.startViewTransition) return;

		return new Promise((resolve) => {
			document.startViewTransition(async () => {
				resolve();
				await navigation.complete;
			});
		});
	});

	$: isActive = (path) => $page.url.pathname === path;
</script>

<nav>
	<ul>
		<li><a href="/" class:active={isActive('/')}>Home</a></li>
		<li><a href="/longer" class:active={isActive('/longer')}>Longer title</a></li>
		<li><a href="/one" class:active={isActive('/one')}>One</a></li>
		<li><a href="/two" class:active={isActive('/two')}>Two</a></li>
	</ul>
</nav>

<slot />

<style>
	ul {
		display: flex;
		justify-content: center;
		gap: 1em;
		margin: 1em 0;
	}

	li {
		display: inline-block;
	}

	.active:after {
		content: '';
		display: inline-block;
		border-bottom: 6px solid red;
		/* position: absolute; */
		width: 100%;
	}

	.active:after {
		view-transition-name: active-page;
	}

	/* :root::view-transition-old(root) { */
	/* 	/1* transform: translateX(30px); *1/ */
	/* } */

	/* :root::view-transition-new(root) { */
	/* 	/1* transform: translateX(-30px); *1/ */
	/* } */
</style>
