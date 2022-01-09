<script lang="ts">
	import { onMount } from 'svelte';

	let shouldLoad = false;
	onMount(() => {
		if (
			!window.localStorage.getItem('gotNameCorrect') ||
			window.localStorage.getItem('gotNameCorrect') !== 'true'
		)
			shouldLoad = false;
		else shouldLoad = true;
	});
</script>

<slot />
