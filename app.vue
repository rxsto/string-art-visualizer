<template>
  <div>
    <canvas id="canvas" :width="w" :height="h"></canvas>
  </div>
</template>

<script lang="ts" setup>
const w = 1000
const h = 1000

const nailCount = 100

onMounted(() => {
  const canvas = document.getElementById('canvas') as HTMLCanvasElement

  const ctx = canvas.getContext('2d')

  if (!ctx) {
    return
  }

  // set canvas background
  ctx.fillStyle = 'rgb(200, 200, 200)'
  ctx.fillRect(0, 0, w, h)

  // draw wooden plate
  ctx.beginPath()
  ctx.arc(w / 2, h / 2, w / 2 - 50, 0, 2 * Math.PI)
  ctx.fillStyle = 'rgb(139,69,19)'
  ctx.fill()

  // calculate index-based coordinates
  const coordinates = []

  for (let i = 0; i < nailCount; i++) {
    const deg = 360 / nailCount * i - 90
    const rad = deg * Math.PI / 180
    const r = w / 2 - 75
    const x = r * Math.cos(rad) + w / 2
    const y = r * Math.sin(rad) + h / 2

    const rLabel = w / 2 - 60
    const xLabel = rLabel * Math.cos(rad) + w / 2
    const yLabel = rLabel * Math.sin(rad) + h / 2

    coordinates.push({ x, y, label: { x: xLabel, y: yLabel } })
  }

  // draw the nails
  for (const [i, coordinatesPair] of Object.entries(coordinates)) {
    const x = coordinatesPair.x
    const y = coordinatesPair.y
    ctx.beginPath()
    ctx.arc(x, y, 4, 0, 2 * Math.PI)
    ctx.fillStyle = 'rgb(105,105,105)'
    ctx.fill()

    const xLabel = coordinatesPair.label.x
    const yLabel = coordinatesPair.label.y
    ctx.fillStyle = 'rgb(0, 0, 0, 0.75)'
    ctx.font = '12px serif'
    ctx.textAlign = 'center'
    ctx.textBaseline = 'middle'
    ctx.fillText(`${i}`, xLabel, yLabel)
  }

  // test drawing randomly
  ctx.beginPath()

  let last = coordinates[Math.floor(Math.random() * coordinates.length)];
  for (let i = 0; i < 500; i++) {
    const next = coordinates[Math.floor(Math.random() * coordinates.length)];
    ctx.moveTo(last.x, last.y)
    ctx.lineTo(next.x, next.y)
    last = next
  }

  ctx.stroke()
})

</script>
