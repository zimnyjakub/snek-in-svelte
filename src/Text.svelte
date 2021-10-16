<script>
import { claim_text } from 'svelte/internal';

	import { renderable } from './game.js';
	import { getLines } from "./util.js";

	export let visible = true;
	
	export let color = 'hsl(0, 0%, 100%)';
	export let align = 'center';
	export let baseline = 'middle';
	
	export let text = '';
	export let x = 0;
	export let y = 0;

	export let maxWidth = 200;
	
	export let fontSize = 16;
	export let fontFamily = '-apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica';
	
	renderable(props => {
		const { context, width, height } = props;
		if (visible && text) {
			let lines = getLines(context, text, maxWidth)
			context.fillStyle = color;
			context.font = `${fontSize}px ${fontFamily}`;
			context.textAlign = align;
			context.textBaseline = baseline;		
			if (lines.length === 1) {
				context.fillText(text, x, y);
				return;
			}

			lines.forEach((it, i) => context.fillText(it, x, y+64*i-64));
		}
	});
</script>

<!-- The following allows this component to nest children -->
<slot />
