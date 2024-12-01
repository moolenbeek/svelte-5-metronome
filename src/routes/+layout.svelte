<script lang="ts">
    import type { Snippet } from 'svelte';
    import type { LayoutData } from './$types';
    import '../app.pcss';
    import Navigation from '$lib/components/Navigation.svelte';

    let { data, children }: { data: LayoutData, children: Snippet } = $props();
</script>

<Navigation />
{@render children()}