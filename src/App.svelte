<script>

	import _ from "lodash"
	import BigPicture from "./BigPicture.svelte"
	
	let big = {}
	
	const queryString = window.location.search
	const urlParams = new URLSearchParams(queryString)
	if (urlParams.has("image")) {
		visaBig(urlParams.get("image"), urlParams.get("size"))
	}

	function visaBig(src,size) {
		document.title = _.last(src.split("\\"))

		big.file = src
		big.size = size

		big.exifState = 0
		big.mouseState = 0

		big.skala = 1
		big.left = 0
		big.top = 100
		big.width  = innerWidth
		big.height = innerHeight

		big.bigWidth = -1
		big.bigHeight = -1
		
		big = big
	}

</script>

<BigPicture bind:big />
