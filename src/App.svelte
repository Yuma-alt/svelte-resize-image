<script>
  import Pica from "pica";

  let imageFile;
  const pica = Pica();
  let width = 200;
  let height = 200;

  async function resizeImage() {
    const reader = new FileReader();
    reader.onload = async (e) => {
      const img = new Image();
      img.src = e.target.result;
      img.onload = () => {
        const canvas = document.createElement("canvas");
        canvas.width = width;
        canvas.height = height;
        pica
          .resize(img, canvas)
          .then((result) => {
            const output = document.getElementById("output");
            output.innerHTML = ""; // 既存の画像をクリア
            output.appendChild(result);
          })
          .catch((error) => console.error(error));
      };
    };
    reader.readAsDataURL(imageFile);
  }
</script>

<div class="container">
  <input type="file" on:change={(e) => (imageFile = e.target.files[0])} />
  <div>
    <label for="width">幅:</label>
    <input type="number" id="width" bind:value={width} min="0" />

    <label for="height">高さ:</label>
    <input type="number" id="height" bind:value={height} min="0" />
  </div>
  <button on:click={resizeImage}>リサイズ</button>

  <div id="output"></div>
</div>

<style>
  .container {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    margin-top: 20px;
  }

  #output {
    margin-top: 20px;
    border: 1px solid #ddd;
    padding: 10px;
  }
</style>
