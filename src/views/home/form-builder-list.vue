<template>

    <div>
        <!-- title -->
        <div class="tab-title">
            <div
                class="tab-title--item"
                :class="{'tab-title--item__active': tabComponent }"
                @click='clickTab(eTab.component)'
            >
                選擇元件
            </div>

            <div
                class="tab-title--item"
                :class="{'tab-title--item__active': tabForm }"
                @click='clickTab(eTab.form)'
            >
                元件屬性
            </div>
        </div>

        <!-- component -->
        <div
            v-show="tabComponent"
            class="form-builder"
            v-for="(listItem, listIndex) in formBuilderElementList"
            :key="listIndex"
        >
            <div class="form-builder--title">
                <img :src="listItem.icon" /> {{ listItem.label }}
            </div>

            <draggable
                class="form-builder--draggable"
                :list="listItem.children"
                :group="{ name: 'formBuilderGroup', pull: 'clone', put: false }"
                :sort="false"
                draggable=".form-builder--list__item"
                @end="dragEndFormBuilder"
            >
                <div
                    v-for="(item, index) in listItem.children"
                    :key="index"
                    class="form-builder--list__item"
                    @click="addFormBuilder(item)"
                >
                    <div class="form-builder--body">
                        <img :src="item.icon" /> {{ item.label }}
                    </div>
                </div>
            </draggable>
        </div>

        <!-- form -->
        <div v-show="tabForm">
            <template v-if="activedItem">
                <template v-if="activedItem.type === eElementType.display_text">
                    <FormBuilderDisplayTextSetting :activedItemData="activedItem" />
                </template>

                <template v-else-if="activedItem.type === eElementType.display_image">
                    <FormBuilderDisplayImageSetting :activedItemData="activedItem" />
                </template>

                <template v-else-if="activedItem.type === eElementType.input_single_text">
                    <FormBuilderInputSingleTextSetting :activedItemData="activedItem" />
                </template>

                <template v-else-if="activedItem.type === eElementType.input_multiple_text">
                    <FormBuilderInputMultipleTextSetting :activedItemData="activedItem" />
                </template>

                <template v-else-if="activedItem.type === eElementType.input_number">
                    <FormBuilderInputNumberSetting :activedItemData="activedItem" />
                </template>

                <template v-else-if="activedItem.type === eElementType.input_text_editor">
                    <FormBuilderInputTextEditorSetting :activedItemData="activedItem" />
                </template>

                <template v-else-if="activedItem.type === eElementType.pick_dropdown_list">
                    <FormBuilderPickDropdownListSetting :activedItemData="activedItem" />
                </template>

                <template v-else-if="activedItem.type === eElementType.pick_radio">
                    <FormBuilderPickRadioSetting :activedItemData="activedItem" />
                </template>

                <template v-else-if="activedItem.type === eElementType.pick_checkbox">
                    <FormBuilderPickCheckboxSetting :activedItemData="activedItem" />
                </template>

                <template v-else-if="activedItem.type === eElementType.pick_switch">
                    <FormBuilderPickSwitchSetting :activedItemData="activedItem" />
                </template>

                <template v-else-if="activedItem.type === eElementType.pick_slider">
                    <FormBuilderPickSliderSetting :activedItemData="activedItem" />
                </template>

                <template v-else-if="activedItem.type === eElementType.pick_date">
                    <FormBuilderPickDateSetting :activedItemData="activedItem" />
                </template>

                <template v-else-if="activedItem.type === eElementType.layout_divider">
                    <FormBuilderLayoutDividerSetting :activedItemData="activedItem" />
                </template>
            </template>
        </div>
    </div>

</template>

<script lang="ts">
//#region Import
//#region Vue
import { Vue, Component, Prop, Watch } from 'vue-property-decorator';
//#endregion

//#region Module
//#endregion

//#region Framework
//#endregion

//#region Src
import { FormBuilderElements, Model as FormBuilderModel } from '@/config';
import { EElementType } from '@/components/form-builders/elements';
//#endregion

//#region Views
import { Model } from './models';
//#endregion

//#region Components
//#endregion

//#region Components Src
import FormBuilderElement from '@/components/form-builders/elements';
//#endregion

//#region Components Views
//#endregion
//#endregion

import draggable from 'vuedraggable';

@Component({
    components: {
        draggable,
        ...FormBuilderElement,
    },
})
export default class FormBuilderList extends Vue {
    //#region Prop
    @Prop({
        type: Array, // Boolean, Number, String, Array, Object
        default: () => [],
    })
    private generateListData: FormBuilderModel.IFormBuilderElement[];

    @Prop({
        type: Object, // Boolean, Number, String, Array, Object
        default: () => {},
    })
    private activedItemData: FormBuilderModel.IFormBuilderElement;
    //#endregion

    @Prop({
        type: String, // Boolean, Number, String, Array, Object
        default: () => Model.ETab.component,
    })
    private currentTabData: Model.ETab;
    //#endregion

    //#region Variables
    private formBuilderElementList: FormBuilderModel.IFormBuilder[] = [];

    private generateList: FormBuilderModel.IFormBuilderElement[] = [];

    private activedItem: FormBuilderModel.IFormBuilderElement = null;

    private currentTab: Model.ETab = Model.ETab.component;

    private eTab = Model.ETab;
    private eElementType = EElementType;
    //#endregion

    //#region Computed
    private get tabComponent(): boolean {
        return this.currentTab === Model.ETab.component;
    }

    private get tabForm(): boolean {
        return this.currentTab === Model.ETab.form;
    }
    //#endregion

    //#region Watch
    @Watch('generateListData', { immediate: true, deep: true })
    private generateListDataChanged(newVal: FormBuilderModel.IFormBuilderElement[], oldVal: FormBuilderModel.IFormBuilderElement[]): void {
        this.generateList = JSON.parse(JSON.stringify(newVal));
        if (newVal.length === 0) {
            this.currentTab = Model.ETab.component;
            this.activedItem = undefined;
        }
    }

    @Watch('activedItemData', { immediate: true, deep: true })
    private activedItemDataChanged(newVal: FormBuilderModel.IFormBuilderElement, oldVal: FormBuilderModel.IFormBuilderElement): void {
        this.activedItem = newVal;
    }

    @Watch('currentTabData', { immediate: true, deep: true })
    private currentTabDataChanged(newVal: Model.ETab, oldVal: Model.ETab): void {
        // this.currentTab = newVal;
    }
    //#endregion

    //#region Vue Life
    private async beforeCreate(): Promise<void> {}
    private async created(): Promise<void> {}
    private async beforeMount(): Promise<void> {
        this.initData();
    }
    private async mounted(): Promise<void> {}
    private async beforeDestroy(): Promise<void> {}
    private async destroyed(): Promise<void> {}
    //#endregion

    //#region Init
    private initData(): void {
        this.formBuilderElementList = FormBuilderElements;
    }
    //#endregion

    //#region View Event
    //#region right
    private addFormBuilder(item: FormBuilderModel.IFormBuilderElement): void {
        let clone: FormBuilderModel.IFormBuilderElement = JSON.parse(JSON.stringify(item));
        clone.id = `form_element_${new Date().getTime()}`;
        this.generateList.push(clone);

        this.$emit('generateList', this.generateList, clone);
    }

    private dragEndFormBuilder(): void {
        let activedItem: FormBuilderModel.IFormBuilderElement = undefined;
        this.generateList.forEach((element) => {
            if (!element.id) {
                element.id = `form_element_${new Date().getTime()}`;
                activedItem = element;
            }
        });

        this.$emit('generateList', this.generateList, activedItem);
    }

    private clickTab(currentTab: Model.ETab): void {
        this.currentTab = currentTab;
    }
    //#endregion
    //#endregion

    //#region Other Function

    //#endregion
}
</script>

<style scoped lang="scss">
</style>
