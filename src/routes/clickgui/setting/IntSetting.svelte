<script lang="ts">
    import "nouislider/dist/nouislider.css";
    import "./nouislider.scss";
    import {createEventDispatcher, onMount} from "svelte";
    import noUiSlider, {type API} from "nouislider";
    import type {IntSetting, ModuleSetting} from "../../../integration/types";
    import ValueInput from "./common/ValueInput.svelte";
    import {convertToSpacedString, spaceSeperatedNames} from "../../../theme/theme_config";

    export let setting: ModuleSetting;

    const cSetting = setting as IntSetting;

    const dispatch = createEventDispatcher();

    let slider: HTMLElement;
    let apiSlider: API;

    onMount(() => {
        // The value can sit outside the declared range (e.g. set through the `.value` command),
        // so widen the slider instead of letting noUiSlider clamp it away.
        const min = Math.min(cSetting.range.from, cSetting.value);
        const max = Math.max(cSetting.range.to, cSetting.value);

        apiSlider = noUiSlider.create(slider, {
            start: cSetting.value,
            connect: "lower",
            range: {
                min,
                max,
            },
            step: 1,
        });

        apiSlider.on("update", (values) => {
            const newValue = parseInt(values[0].toString());

            cSetting.value = newValue;
            setting = { ...cSetting };
        });

        apiSlider.on("set", () => {
            dispatch("change");
        });
    });
</script>

<div class="setting">
    <div class="name">{$spaceSeperatedNames ? convertToSpacedString(cSetting.name) : cSetting.name}</div>
    
    <div class="slider-container">
        <div bind:this={slider} class="slider"></div>
    </div>

    <div class="value-box">
        <ValueInput valueType="int" value={cSetting.value} on:change={(e) => apiSlider.set(e.detail.value)}/>
    </div>
</div>

<style lang="scss">
    .setting {
        display: grid;
        grid-template-columns: 120px 1fr 60px;
        align-items: center;
        padding: 4px 0;
        gap: 8px;
    }

    .name {
        font-size: 12px;
        font-weight: 500;
        color: var(--clickgui-text-color);
        white-space: nowrap;
        overflow: hidden;
        text-overflow: ellipsis;
    }

    .slider-container {
        display: flex;
        align-items: center;
        width: 100%;
        padding: 0 4px;
    }

    .slider {
        width: 100%;
    }

    .value-box {
        display: flex;
        align-items: center;
        justify-content: center;
        background: var(--clickgui-window-background-color);
        border: 1px solid var(--clickgui-border-color);
        border-radius: 8px;
        padding: 6px 0px;
        width: 100%;
    }

    :global(.value-box .value) {
        font-size: 12px;
        font-weight: 400;
        color: var(--clickgui-text-color);
        font-variant-numeric: tabular-nums;
    }
</style>
