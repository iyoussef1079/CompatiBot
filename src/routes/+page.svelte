<script lang="ts">
    import { CodeBlock, ListBox, ListBoxItem, getToastStore, type ToastSettings } from '@skeletonlabs/skeleton';

    const toastStore = getToastStore();

    let gpus = ["NVIDIA RTX 3080", "NVIDIA RTX 3070", "AMD RX 6800"];
    let pytorchVersions = ["1.8.0", "1.9.0", "1.10.0"];
    let cudaVersions = ["11.1", "11.2", "11.3"];

    let selectedGPU: string | null = null;
    let selectedPyTorch: string | null = null;
    let selectedCuda: string | null = null;
    let displayCode: string | null = null;

    function checkCompatibility() {
        if (selectedGPU == null || selectedPyTorch == null || selectedCuda == null) {
            const t: ToastSettings = {
                message: 'Please select a GPU, PyTorch version, and Cuda version',
                background: 'variant-filled-error',
            };
            toastStore.trigger(t);
        } else {
            fetchCodeFromAPI(selectedGPU, selectedPyTorch, selectedCuda);
        }
    }

    async function fetchCodeFromAPI(gpu: string, pytorch: string, cuda: string) {
        const apiUrl = '/api/get-compatibility-code';
        try {
            const response = await fetch(apiUrl, {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json'
                },
                body: JSON.stringify({ gpu, pytorch, cuda })
            });

            if (!response.ok) {
                throw new Error('An unexpected error occurred while verifying compatibility.');
            }

            const data = await response.json();
            displayCode = data.code;
        } catch (error: any) {
            toastStore.trigger({
                message: error.message || 'An unexpected error occurred while verifying compatibility.',
                background: 'variant-filled-error',
            });
            displayCode = null;
        }
    }
</script>

<div class="flex flex-col items-center justify-center min-h-screen">
    <div class="grid grid-cols-3 gap-4 mb-8">
            <div class="flex flex-col">
                <label class="label">
                    <span>Select a GPU</span>
                    <select class="select" bind:value={selectedGPU}>
                        {#each gpus as gpu}
                            <option>{gpu}</option>
                        {/each}
                    </select>
                </label>
            </div>

            <div class="flex flex-col">
                <label class="label">
                    <span>Select a Pytorch version</span>
                    <select class="select" bind:value={selectedPyTorch}>
                        {#each pytorchVersions as torch_version}
                            <option>{torch_version}</option>
                        {/each}
                    </select>
                </label>
            </div>

            <div class="flex flex-col">
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

    {#if displayCode}
        <CodeBlock language="shell" code={displayCode}></CodeBlock>
    {/if}
</div>
