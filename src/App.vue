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
const amplitude = ref(150);
const frequency = ref(1);
const phase = ref(0);

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

        const fv = type.value === WaveType.Sin ? Math.sin(counter + phase.value) : Math.cos(counter + phase.value);

        y = 360 - fv * amplitude.value;
        counter += increase;

        c.lineTo(x, y);
        c.stroke();
    }
};

watch(type, draw);
watch(amplitude, draw);
watch(frequency, draw);
watch(phase, draw);

onMounted(draw);
</script>

<template>
    <div class="flex flex-col p-4 gap-4 h-full">
        <div class="text-2xl font-semibold">Waves</div>
        <div class="flex gap-4 justify-between items-center">
            <div class="flex gap-2 items-center">
                Type:
                <select v-model="type">
                    <option value="sin">Sine</option>
                    <option value="cos">Cosine</option>
                </select>
            </div>
            <div class="flex gap-2">
                Amplitude:
                <input v-model.number="amplitude" class="w-full" type="range" min="5" max="300">
                ({{ amplitude }})
            </div>
            <div class="flex gap-2">
                Frequency:
                <input v-model.number="frequency" class="w-full" type="range" min="1" max="20">
                ({{ frequency }})
            </div>
            <div class="flex gap-2">
                Phase:
                <input v-model.number="phase" class="w-full" type="range" min="0" max="20">
                ({{ phase }})
            </div>
        </div>
        <div class="flex items-center justify-center flex-1">
            <canvas ref="canvas" width="720px" height="720px" class="border border-black" />
        </div>
    </div>
</template>
