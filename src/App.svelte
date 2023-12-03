<script>
  import Pica from "pica";
  import heic2any from "heic2any";

  let imageFile;
  const pica = Pica();
  let width = 200;
  let height = 200;

  async function convertHEICToJPEG(file) {
    try {
      const convertedBlob = await heic2any({
        blob: file,
        toType: "image/jpeg",
      });
      return convertedBlob;
    } catch (e) {
      console.error(e);
      return null;
    }
  }

  async function handleFileChange(e) {
    let file = e.target.files[0];

    if (file.type === "image/heic") {
      const convertedBlob = await convertHEICToJPEG(file);
      if (convertedBlob) {
        file = new File([convertedBlob], "converted.jpeg", {
          type: "image/jpeg",
        });
      }
    }

    imageFile = file;
    resizeImage();
  }

  async function resizeImage() {
    if (!imageFile) {
      return;
    }

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
  <input type="file" on:change={handleFileChange} />
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
