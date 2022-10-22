<script>
	export let big

	let exif

	const round = (x,n) => Math.round(x*Math.pow(10,n))/Math.pow(10,n)

	function getExif() {
		const img = document.getElementById("picture")
		big.bigWidth  = img.naturalWidth
		big.bigHeight = img.naturalHeight
		big.exifState = 1
		EXIF.getData(img, function() {
			exif = EXIF.getAllTags(this)
			if (exif.ExifVersion) {
				big.exifState = 2
				exif.DateTimeOriginal = exif.DateTimeOriginal.replace(":","-").replace(":","-")
			}
		})
	}

	function wheel(e) {
		e.preventDefault()
		e.stopPropagation()

		const f = (skala,left,x) => (1-skala) * (x-left)
	
		let faktor = 1.1
		if (e.deltaY > 0) faktor = 1/1.1
		big.left += f(faktor,big.left,e.x)
		big.top  += f(faktor,big.top,e.y)
		big.skala *= faktor
		big.width = big.skala * big.bigWidth
	}

	function mousedown(e) {
		e.preventDefault()
		e.stopPropagation()
		big.mouseState = 1
		big.startX = e.x
		big.startY = e.y
	}

	function mousemove(e) {
		if (big.mouseState != 1) return
		big.left += e.x - big.startX
		big.top += e.y - big.startY
		big.startX = e.x
		big.startY = e.y
	}

	const mouseup = () => big.mouseState = 0
	const mouseout = () => big.mouseState = 0

</script>

<span style="top:1%">{big.file.replaceAll('\\',' • ').replaceAll("_"," ")}</span>
{#if big.exifState >= 1}
	<span style="top:3%"> {round(big.bigWidth * big.bigHeight/1000000,1)} MP • {big.bigWidth} x {big.bigHeight} • {round(big.bigSize/1000000,1)} MB</span>
{/if}
{#if big.exifState == 2}
	<span style="top:5%;"> {exif.DateTimeOriginal.replace(" "," • ")} </span>
	<span style="top:7%;"> {exif.Model} • f/{exif.FNumber} • 1/{1/exif.ExposureTime} • {exif.FocalLength} mm • ISO {exif.ISOSpeedRatings} </span>
	<span style="top:9%;"> © {exif.Copyright} </span>
{/if}

<button style = "left:0.5%;  top:0.5%;" on:click={()=>getExif()}>i</button>

<img id='picture' src={big.file} alt=""
	on:wheel = {wheel}
	on:mousedown = {mousedown}
	on:mousemove = {mousemove}
	on:mouseup = {mouseup}
	on:mouseout = {mouseout}
	on:blur = {blur}
	width = {big.width}
	style = "position:absolute; left:{big.left}px; top:{big.top}px;"
>

<style>
	button {
		border: 1px solid grey;
		color : grey;
		background-color: transparent; 
		position:absolute; 
		font-size:1em;
		width:4%;
	}

	span {
		position:absolute;
		left:5%;
	}

</style>
