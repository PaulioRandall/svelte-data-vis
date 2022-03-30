<script>
	import Body from '/src/shared/svelte/page/Body.svelte'

	const title = 'Svelte + Data Viz'

	const theme = {
		points: {
			size: 6,
			color: 'black',
			fontSize: '16px',
		},
		axis: {
			size: 3,
			color: 'black',
			fontSize: '24px',
		},
	}

	const data = [
		{ x: 100, y: 100, label: 'Bread' },
		{ x: 140, y: 150, label: 'Cheese' },
		{ x: 165, y: 165, label: 'Milk' },
		{ x: 225, y: 230, label: 'Chocolate' },
		{ x: 240, y: 250, label: 'Apple' },
		{ x: 265, y: 280, label: 'Soup' },
		{ x: 290, y: 305, label: 'Butter' },
		{ x: 325, y: 310, label: 'Garlic' },
		{ x: 350, y: 350, label: 'Jam' },
		{ x: 370, y: 365, label: 'Lettuce' },
		{ x: 410, y: 400, label: 'Salt' },
		{ x: 425, y: 450, label: 'Chicken' },
		{ x: 455, y: 480, label: 'Beef' },
	]

	const graph = {
		w: 800,
		h: 600,
		pad: 40,
	}

	const axis = {
		topLeft: `${graph.pad}, ${graph.pad}`,
		bottomLeft: `${graph.pad}, ${graph.h - graph.pad}`,
		bottomRight: `${graph.w - graph.pad}, ${graph.h - graph.pad}`,

		xLabel: {
			x: graph.pad * 2,
			y: graph.h - graph.pad / 4,
			text: 'Time',
		},
		yLabel: {
			x: graph.pad / 2,
			y: graph.h - graph.pad * 3,
			text: 'Shine',
		},

		points: function () {
			return `${this.topLeft} ${this.bottomLeft} ${this.bottomRight}`
		},
	}

	const mapPoint = (p) => {
		return {
			x: p.x,
			y: graph.h - graph.pad - p.y,
			label: p.label,
		}
	}
</script>

<svelte:head>
	<title>{title}</title>
</svelte:head>

<Body>
	<h1 class="page-heading">
		{title}
	</h1>
	<svg width="{graph.w}" height="{graph.h}">
		{#each data as point}
			{@const p = mapPoint(point)}
			<g>
				<circle
					class="data-point"
					cx="{p.x}"
					cy="{p.y}"
					r="{theme.points.size}"
					fill="{theme.points.color}"
					data-label="{p.label}"></circle>
				<text
					class="data-point-label"
					x="{p.x + 20}"
					y="{p.y + 20}"
					fill="{theme.points.color}"
					font-size="{theme.points.fontSize}">
					{p.label}
				</text>
			</g>
		{/each}

		<polyline
			points="{axis.points()}"
			fill="none"
			stroke="{theme.axis.color}"
			stroke-width="{theme.axis.size}"></polyline>
		<text
			x="{axis.xLabel.x}"
			y="{axis.xLabel.y}"
			fill="{theme.axis.color}"
			font-size="{theme.axis.fontSize}">
			{axis.xLabel.text}
		</text>
		<text
			x="{axis.yLabel.x}"
			y="{axis.yLabel.y}"
			text-anchor="middle"
			dominant-baseline="central"
			transform="rotate(-90, {axis.yLabel.x}, {axis.yLabel.y})"
			fill="{theme.axis.color}"
			font-size="{theme.axis.fontSize}">
			{axis.yLabel.text}
		</text>
	</svg>
</Body>

<style>
	@import '/src/shared/css/base.css';

	svg {
		background-color: rgba(0, 0, 0, 0.1);
	}

	.data-point-label {
		opacity: 0;
		cursor: default;
	}

	.data-point:hover + .data-point-label {
		opacity: 1;
	}
</style>
