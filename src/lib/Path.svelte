<!--
@component
The Path component needs to be placed either inside a svelte-konva Layer or Group component. 

### Usage:
```tsx
<Path config={{ x: 100, y: 100, width: 100, height: 100, fill: "blue", data: "M213.1,6.7c-32.4-14.4-73.7,0-88.1,30.6C110.6,4.9,67.5-9.5,36.9,6.7C2.8,22.9-13.4,62.4,13.5,110.9C33.3,145.1,67.5,170.3,125,217c59.3-46.7,93.5-71.9,111.5-106.1C263.4,64.2,247.2,22.9,213.1,6.7z" }} />
```

Further information: [Konva API docs](https://konvajs.org/api/Konva.Path.html), [svelte-konva docs](https://teykey1.github.io/svelte-konva)
-->
<script lang="ts">
	/**
	 * WARNING: This component is autogenerated using the svelteKonvaComponent.hbs template. Do not edit this file manually!
	 */

	import Konva from 'konva';
	import { onMount, onDestroy, createEventDispatcher } from 'svelte';
	import type { Writable } from 'svelte/store';
	import { registerEvents } from '$lib/util/events';
	import { getParentContainer, type KonvaParent } from '$lib/util/manageContext';
	import { copyExistingKeys } from '$lib/util/copy';

	export let config: Konva.PathConfig;
	export let handle = new Konva.Path(config);

	let parent: Writable<null | KonvaParent> = getParentContainer();
	let dispatcher = createEventDispatcher();

	$: handle.setAttrs(config);

	onMount(() => {
		$parent!.add(handle);

		handle.on('transformend', () => {
			copyExistingKeys(config, handle.getAttrs());
			config = config;
		});

		handle.on('dragend', () => {
			copyExistingKeys(config, handle.getAttrs());
			config = config;
		});

		registerEvents(dispatcher, handle);
	});

	onDestroy(() => {
		handle.destroy();
	});
</script>
