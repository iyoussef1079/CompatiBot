<script lang="ts">
    import { ListBox, ListBoxItem } from '@skeletonlabs/skeleton';

    let gpus = ["NVIDIA RTX 3080", "NVIDIA RTX 3070", "AMD RX 6800"];
    let pytorchVersions = ["1.8.0", "1.9.0", "1.10.0"];
    let cudaVersions = ["11.1", "11.2", "11.3"];

    let selectedGPU = '';
    let selectedPyTorch = '';
    let selectedCuda = '';

    function checkCompatibility() {
        console.log(selectedGPU, selectedPyTorch, selectedCuda);
    }
</script>

<div class="">
    <div class="">

        <div class="">
            <label class="label">
                <span>Select a GPU</span>
                <select class="select" bind:value={selectedGPU}>
                    {#each gpus as gpu}
                        <option>{gpu}</option>
                    {/each}
                </select>
            </label>
        </div>

        <div class="">
            <label class="label">
                <span>Select a Pytorch version</span>
                <select class="select" bind:value={selectedPyTorch}>
                    {#each pytorchVersions as torch_version}
                        <option>{torch_version}</option>
                    {/each}
                </select>
            </label>
        </div>

        <div class="">
            <label class="label">
                <span>Select a Cuda version</span>
                <select class="select" bind:value={selectedCuda}>
                    {#each cudaVersions as cuda}
                        <option>{cuda}</option>
                    {/each}
                </select>
            </label>
        </div>

    </div>

    <label class="label">
        <button type="button" class="btn variant-filled" on:click={checkCompatibility}>
            <span>Test Compatibility</span>
        </button>
    </label>
</div>
