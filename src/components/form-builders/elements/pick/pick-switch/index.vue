<template>
    <b-row>

        <b-col cols="2">
            <div class="d-flex flex-column justify-content-center align-items-end w-100 h-100"> switch </div>
        </b-col>

        <b-col cols="10">
            <DeleteCopy
                v-if="isActived"
                @actionCopy="actionCopy"
                @actionDelete="actionDelete"
            />

            <toggle-button
                v-model="model"
                :height='34'
                :width='210'
                :font-size='16'
                :labels="{checked: 'open', unchecked: 'close'}"
                :color=" {checked: '#82C7EB', unchecked: '#BFCBD9'}"
                :sync='true'
            />

        </b-col>
    </b-row>
</template>

<script lang="ts">
//#region Import
//#region Vue
import { Vue, Component, Prop } from 'vue-property-decorator';
//#endregion

//#region Module
//#endregion

//#region Framework
//#endregion

//#region Src
import { Model } from '@/config/index';
//#endregion

//#region Views
//#endregion

//#region Components
//#endregion

//#region Components Src
import DeleteCopy from '@/components/form-builders/action/delete-copy.vue';
//#endregion

//#region Components Views
//#endregion

//#region toggle-button
import { ToggleButton } from 'vue-js-toggle-button';
//#endregion
//#endregion

@Component({
    components: { DeleteCopy, ToggleButton },
})
export default class ComponentElement extends Vue {
    //#region Prop
    @Prop({
        type: Boolean, // Boolean, Number, String, Array, Object
        default: () => false,
    })
    private isActived: boolean;

    @Prop({
        type: Number, // Boolean, Number, String, Array, Object
        default: () => undefined,
    })
    private index: number;

    @Prop({
        type: Object, // Boolean, Number, String, Array, Object
        default: () => undefined,
    })
    private activedItemData: Model.IFormBuilderElement;
    //#endregion

    //#region Variables
    model: { value: string; text: string } = null;
    options: { value: string; text: string }[] = [
        { value: '1', text: 'one' },
        { value: '2', text: 'two' },
    ];
    //#endregion

    //#region Computed
    private get activedItem(): Model.IFormBuilderElement {
        return this.activedItemData;
    }
    //#endregion

    //#region Watch
    //#endregion

    //#region Vue Life
    private async beforeCreate(): Promise<void> {}
    private async created(): Promise<void> {}
    private async beforeMount(): Promise<void> {}
    private async mounted(): Promise<void> {}
    private async beforeDestroy(): Promise<void> {}
    private async destroyed(): Promise<void> {}
    //#endregion

    //#region Init
    //#endregion

    //#region View Event
    private actionCopy(): void {
        this.$emit('actionCopy', this.activedItem, this.index);
    }

    private actionDelete(): void {
        this.$emit('actionDelete', this.activedItem, this.index);
    }
    //#endregion

    //#region Other Function
    //#endregion
}
</script>

<style scoped lang="scss">
::v-deep .vue-js-switch {
    margin: 0;
}
</style>