<script setup lang="ts">
/**
 * Waves
 *
 * @author      Afaan Bilal
 * @link        https://afaan.dev
 * @link        https://github.com/AfaanBilal/waves
 * @license     MIT
 * @copyright   2023 Afaan Bilal
 */

import { onMounted, ref, watch } from "vue";

enum WaveType {
    Sin = "sin",
    Cos = "cos",
}

const canvas = ref<HTMLCanvasElement>();

const type = ref<WaveType>(WaveType.Sin);
const amplitude = ref(60);
const frequency = ref(3);

const draw = () => {
    if (!canvas.value) return;

    const c: CanvasRenderingContext2D = canvas.value.getContext("2d")!;
    c.clearRect(0, 0, canvas.value.width, canvas.value.height);

    c.beginPath();

    let i;
    for (i = 0; i < 720; i += 20) {
        c.moveTo(i + 5, 360);
        c.lineTo(i, 360);
    }

    c.stroke();

    let counter = 0, x = 0, y = 360;

    //100 iterations
    let increase = 90 / 360 * Math.PI / 9;
    for (i = 0; i <= 720; i += 10 / frequency.value) {
        c.moveTo(x, y);
        x = i;

        const fv = type.value === WaveType.Sin ? Math.sin(counter) : Math.cos(counter);

        y = 360 - fv * amplitude.value;
        counter += increase;

        c.lineTo(x, y);
        c.stroke();
    }
};

watch(type, draw);
watch(amplitude, draw);
watch(frequency, draw);

onMounted(draw);
</script>

<template>
    <div class="flex flex-col">
        <div class="text-2xl">Waves</div>
        <div class="flex gap-1">
            Type:
            <select v-model="type">
                <option value="sin">Sine</option>
                <option value="cos">Cosine</option>
            </select>
        </div>
        <div>
            Amplitude: ({{ amplitude }}) <input v-model.number="amplitude" class="w-full" type="range" min="10" max="200">
        </div>
        <div>
            Frequency: ({{ frequency }}) <input v-model.number="frequency" class="w-full" type="range" min="1" max="20">
        </div>
        <canvas ref="canvas" width="1000px" height="1000px" class="border" />
    </div>
</template>
