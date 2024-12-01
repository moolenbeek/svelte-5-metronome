<script lang="ts">
	import { Button } from "$lib/components/ui/button";
</script>

<nav class="navigation border-b">
	<div class="container flex h-14 items-center px-4 max-w-7xl mx-auto">
		<h1 class="text-lg font-semibold"><a href="/">Website</a></h1>
		
		<div class="flex items-center gap-4 ml-auto">
			<Button variant="ghost" href="/about">About</Button>
			<Button variant="ghost" href="/contact">Contact</Button>
		</div>
	</div>
</nav>
