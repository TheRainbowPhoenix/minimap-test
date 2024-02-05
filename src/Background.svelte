<script>
  import { renderable } from "./game.js";
  export let color = null;
  export let backgroundImage = "./map-bg-ai.png"; // Specify the path to your background image

  const loadImage = (src) => {
    return new Promise((resolve, reject) => {
      const image = new Image();
      image.onload = () => resolve(image);
      image.onerror = reject;
      image.src = src;
    });
  };

  let image;

  loadImage(backgroundImage).then((i) => {
    image = i;
  });

  renderable(async (props) => {
    const { context, width, height } = props;

    context.clearRect(0, 0, width, height);

    // Draw background color if specified
    if (color) {
      context.fillStyle = color;
      context.fillRect(0, 0, width, height);
    }

    // Draw background image
    if (image) {
      context.drawImage(image, 0, 0, image.width, image.height);
    }
  });
</script>

<!-- The following allows this component to nest children -->
<slot />
