<template>
  <input
    style="margin-bottom: 25px"
    accept="image/jpeg, image/png"
    type="file"
    @change="onFileInput"
  />
  <div v-if="base64 && width && height">
    <img style="max-width: 500px; margin-bottom: 25px" :src="base64" />
    <div><b>Width:</b> {{ width }}</div>
    <div><b>Height:</b> {{ height }}</div>
    <div><b>Cover width:</b> {{ Math.round(coverWidth) }}</div>
    <div><b>Has spine:</b> {{ hasSpine ? "yes" : "no" }}</div>
    <div><b>Spine width:</b> {{ hasSpine ? Math.round(spineWidth) : "-" }}</div>

    <div style="margin-top: 25px">
      <div><b>Aspect ratio</b></div>
      <input
        placeholder="Set aspect ratio"
        v-model.number="ratio"
        type="text"
        style="margin-bottom: 25px"
      />
      <button @click="extract">Update</button>
    </div>
    <div style="margin-top: 25px">
      <div><b>Pixel offsets</b></div>
      <input
        placeholder="Set offset left"
        v-model.number="offsetLeft"
        type="number"
        style="margin-bottom: 25px; margin-right: 10px; width: 100px"
      />
      <input
        v-if="hasSpine"
        placeholder="Set offset right"
        v-model.number="offsetRight"
        type="number"
        style="margin-bottom: 25px; width: 100px"
      />
    </div>

    <h3 style="margin-top: 25px; margin-bottom: 5px">Result</h3>
    <div style="margin-bottom: 25px; opacity: 0.75">
      Right-click -> save image as
    </div>
    <div
      style="
        padding: 10px;
        background: green;
        display: flex;
        justify-content: center;
      "
    >
      <img
        v-for="base64 in generatedBase64"
        :key="base64"
        style="margin-right: 10px; max-height: 500px"
        :src="base64"
      />
    </div>
  </div>

  <canvas id="canvas" v-show="false"></canvas>
</template>

<script lang="ts">
import { ref, defineComponent, computed } from "vue";

function cropSeries(
  img: HTMLImageElement,
  widths: number[],
  height: number
): string[] {
  const images: string[] = [];

  let x = 0;

  for (const width of widths) {
    images.push(cropImage(img, [x, 0, width, height]));
    x += width;
  }

  return images;
}

function cropImage(
  img: HTMLImageElement,
  crop: [number, number, number, number]
): string {
  const canvas = document.getElementById("canvas") as HTMLCanvasElement;
  const context = canvas.getContext("2d")!;

  const [_x, _y, width, height] = crop;

  canvas.width = width;
  canvas.height = height;

  console.log("Cropping image:", crop);

  context.drawImage(img, ...crop, 0, 0, width, height);
  return canvas.toDataURL();
}

export default defineComponent({
  name: "Cropper",
  setup() {
    const base64 = ref<string | null>(null);
    const width = ref(-1);
    const height = ref(-1);
    const ratio = ref(0.706);

    const coverWidth = computed(() => height.value * ratio.value);
    const hasSpine = computed(() => width.value > coverWidth.value * 2);
    const spineWidth = computed(() => width.value - coverWidth.value * 2);

    const offsetLeft = ref(0);
    const offsetRight = ref(0);

    const generatedBase64 = ref<string[]>([]);

    function extract() {
      if (base64.value) {
        const img = new Image();

        img.onload = () => {
          generatedBase64.value = [];

          width.value = img.width;
          height.value = img.height;

          const widths: number[] = [];

          // First cover
          widths.push(coverWidth.value + offsetLeft.value);

          // Spine
          if (hasSpine.value) {
            widths.push(spineWidth.value - offsetRight.value);
          }

          // Second cover
          widths.push(coverWidth.value);

          generatedBase64.value = cropSeries(img, widths, height.value);
        };

        img.src = base64.value;
      }
    }

    function onFileInput(event: Event) {
      const target = event.target as HTMLInputElement;
      const file = target!.files![0];

      if (file) {
        const reader = new FileReader();
        reader.addEventListener("load", () => {
          // convert image file to base64 string
          base64.value = reader.result?.toString() || null;
          extract();
        });

        reader.readAsDataURL(file);
      }
    }

    return {
      base64,
      onFileInput,

      width,
      height,

      ratio,

      coverWidth,
      hasSpine,
      spineWidth,

      generatedBase64,

      extract,

      offsetLeft,
      offsetRight,
    };
  },
});
</script>

<style scoped></style>
