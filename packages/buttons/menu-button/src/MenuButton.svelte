<script lang="ts">
	// Base
	import { DOMEventsForwarder } from "@svelte-material-design/core/common/events";
	const forwardDOMEvents = DOMEventsForwarder();
	let className = "";
	export { className as class };
	export let style: string = null;

	export let dom: HTMLButtonElement = null;
	import type { BaseProps } from "@svelte-material-design/core/common/dom/Props";
	export let props: BaseProps = {};

	// MenuButton
	import { Button } from "@svelte-material-design/core/button";
	import type { Menu } from "@svelte-material-design/core/menu";
	import { DefaultFocusState } from "@material/menu";

	export let ripple: boolean = true;
	export let color: "primary" | "secondary" = "primary";
	export let variant: "raised" | "unelevated" | "outlined" = null;
	export let density: number = undefined;
	export let disabled: boolean = false;

	export let menu: any;

	function handleKey(event: CustomEvent<KeyboardEvent>) {
		if (!menu) return;

		if (
			event.detail.key === " " ||
			event.detail.key === "ArrowDown" ||
			event.detail.key === "Enter"
		) {
			menu.setDefaultFocusState(DefaultFocusState.FIRST_ITEM);
			menu.setOpen(true);
		}

		if (event.detail.key === "ArrowUp") {
			menu.setDefaultFocusState(DefaultFocusState.LAST_ITEM);
			menu.setOpen(true);
		}
	}

	function handleClick(event: CustomEvent<MouseEvent>) {
		if (!menu) return;

		menu.setDefaultFocusState(DefaultFocusState.LIST_ROOT);
		menu.setOpen(true);
	}
</script>

<Button
	props={{ ...props }}
	bind:dom
	class={className}
	{style}
	on:domEvent={forwardDOMEvents}
	on:keyup={handleKey}
	on:click={handleClick}
	{ripple}
	{color}
	{variant}
	{density}
	{disabled}>
	<slot />
</Button>

<!-- <MenuSurface
  bind:element
  use={[...use]}
  class="mdc-menu {className}"
  on:MDCMenu:selected={updateOpen}
  on:MDCMenuSurface:closed={updateOpen}
  on:MDCMenuSurface:opened={updateOpen}
  {...exclude($$props, ['use', 'class', 'wrapFocus'])}>
  <slot />
</MenuSurface> -->
