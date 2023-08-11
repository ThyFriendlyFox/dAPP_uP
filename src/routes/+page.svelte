<script>
	import { onMount } from 'svelte';


	onMount(async () => {
		// Get the canvas and context
		const canvas = document.getElementById('canvas');
		const ctx = canvas.getContext('2d');

		// Set the canvas size to the size of the window
		function initCanvas() {
			canvas.width = window.innerWidth;
			canvas.height = window.innerHeight;
		}

		// Initialize the WebGPU context
		function initWebGPU() {
			// This function is left blank as the WebGPU API is not yet available in all browsers
			// When it is, this function can be used to initialize the WebGPU context
		}

		// Start drawing when the left mouse button is pressed
		function startDrawing(event) {
			drawing = true;
			draw(event);
		}

		// Stop drawing when the left mouse button is released
		function stopDrawing(event) {
			if (!drawing) return;
			drawing = false;
			paths.push(currentPath);
			currentPath = [];
		}

		// Draw a line or dot at the current mouse position
		function draw(event) {
			if (!drawing) return;
			ctx.beginPath();
			ctx.arc(event.clientX, event.clientY, 5, 0, Math.PI * 2);
			ctx.fill();
			currentPath.push({x: event.clientX, y: event.clientY});
		}

		// Clear the most recent drawing from the canvas
		function clearCanvas() {
			if (paths.length > 0) {
				ctx.clearRect(0, 0, canvas.width, canvas.height);
				paths.forEach(path => {
					path.forEach(point => {
						ctx.beginPath();
						ctx.arc(point.x, point.y, 5, 0, Math.PI * 2);
						ctx.fill();
					});
				});
			}
		}

		// Initialize the canvas and WebGPU context
		initCanvas();
		initWebGPU();

		// Add event listeners for the mouse events
		canvas.addEventListener('mousedown', startDrawing);
		canvas.addEventListener('mouseup', stopDrawing);
		canvas.addEventListener('mousemove', draw);

		// Add an event listener for the CTRL+Z key combination
		document.addEventListener('keydown', function(event) {
			if (event.ctrlKey && event.key === 'z') {
				clearCanvas();
			}
		});

		// Variables to keep track of the drawing state
		let drawing = false;
		/**
		 * @type {{ x: any; y: any; }[]}
		 */
		let currentPath = [];
		/**
		 * @type {{ x: any; y: any; }[][]}
		 */
		let paths = [];

		
	});
</script>

<div>
    <canvas id="canvas"></canvas>
</div>

<style>
	body {
		margin: 0;
		overflow: hidden;
	}
	canvas {
		display: block;
		width: 100vw;
		height: 100vh;
	}
</style>