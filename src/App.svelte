<script>

	import _ from "lodash"
	import BigPicture from "./BigPicture.svelte"
	
	const range = _.range
	let big = {}
	
	const queryString = window.location.search
	const urlParams = new URLSearchParams(queryString)
	if (urlParams.has("image")) {
		visaBig(innerWidth, innerHeight, urlParams.get("bigSize"), urlParams.get("image"))
	}

	function visaBig(width,height,bigSize,src) {
		document.title = _.last(src.split("\\"))

		big.exifState = 0
		big.mouseState = 0

		big.skala = 1
		big.left = 0
		big.top = 100

		big.bigWidth = innerWidth
		big.bigHeight = innerHeight

		big.width  = big.bigWidth
		big.height = big.bigHeight
		
		big.file = src
		big.bigSize = bigSize
		big = big
	}

</script>

<BigPicture bind:big />
