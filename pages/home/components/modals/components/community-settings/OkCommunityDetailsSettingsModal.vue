<template>
    <div class="is-flex justify-center align-items-center">
        <ok-community-details-settings
            class="ok-community-details-settings-modal"
            :images="params && params.images"
            :community="params && params.community"
            @onSaveComplete="onSaveComplete"
        />
    </div>
</template>

<style lang="scss" scoped>
    .ok-community-details-settings-modal {
        width: 540px;

        @media only screen and ( max-width: 540px ) {
            width: auto;
        }
    }
</style>

<script lang="ts">
    import { Component, Prop, Vue } from 'nuxt-property-decorator';
    import OkCommunityDetailsSettings from '~/components/settings/community-settings/community-details/OkCommunityDetailsSettings.vue';
    import { CommunityDetailsSettingsModalParams } from '~/services/modal/IModalService';

    @Component({
        name: 'OkCommunityDetailsSettingsModal',
        components: { OkCommunityDetailsSettings }
    })
    export default class OkCommunityDetailsSettingsModal extends Vue {
        @Prop({
            type: Function,
            required: false
        }) readonly returnDataSetter: (data: any) => void;

        @Prop({
            type: Object,
            required: false
        }) readonly params: CommunityDetailsSettingsModalParams;

        onSaveComplete() {
            this.$parent['close']();
        }
    }
</script>
