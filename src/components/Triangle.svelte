<script lang="ts">
    export let width = 200;
    $: height = width * 0.896;
</script>

<svg class=" focus:outline-0 t" width={width} height={height} viewBox="0 0 {width} {height}" xmlns="http://www.w3.org/2000/svg">
  <!-- 枠いっぱいに表示される下向きの正三角形 -->
  <polygon points="0,0 {width},0 {width / 2},{height}"/>
</svg>