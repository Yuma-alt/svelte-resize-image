<script>
  import Pica from "pica";

  let imageFile;
  const pica = Pica();

  async function resizeImage() {
    const reader = new FileReader();
    reader.onload = async (e) => {
      const img = new Image();
      img.src = e.target.result;
      img.onload = () => {
        const canvas = document.createElement("canvas");
        canvas.width = 200; // width after resize
        canvas.height = 200; // height after resize
        pica
          .resize(img, canvas)
          .then((result) => document.body.appendChild(result))
          .catch((error) => console.error(error));
      };
    };
    reader.readAsDataURL(imageFile);
  }
</script>

<input type="file" on:change={(e) => (imageFile = e.target.files[0])} />
<button on:click={resizeImage}>リサイズ</button>
