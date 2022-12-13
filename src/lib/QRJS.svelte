<script>
  // @ts-nocheck
  import { onMount } from "svelte";

  export let codeValue;
  export let squareSize;

  let qrcode;

  onMount(() => {
    let script = document.createElement("script");
    script.src = "src/lib/qrcode.min.js";
    document.head.append(script);

    script.onload = function () {
      qrcode = new QRCode("qrcode", {
        text: codeValue,
        width: squareSize,
        height: squareSize,
        colorDark: "#000000",
        colorLight: "#ffffff",
        correctLevel: QRCode.CorrectLevel.H,
      });
    };
  });
</script>

<div id="qrcode" />

<style>
  #qrcode {
    width: 20px;
    height: 200x;
    margin-top: 15px;
  }
</style>
