<template>
    <div class="card ok-has-background-primary ok-desktop-community-profile-sidebar">
        <div class="card-header ok-has-border-bottom-primary-highlight" v-if="headerVisible">
            <div class="columns has-padding-left-20 has-padding-right-20 has-padding-top-10 has-padding-bottom-10 has-width-100-percent is-marginless">
                <div class="column is-narrow is-flex align-items-center">
                    <ok-community-avatar :community="community"></ok-community-avatar>
                </div>
                <div class="column">
                    <div class="columns is-gapless is-marginless flex-direction-column">
                        <div class="column">
                            <ok-mobile-community-profile-title :community="community"></ok-mobile-community-profile-title>
                        </div>
                        <div class="column">
                            <ok-mobile-community-profile-name :community="community"></ok-mobile-community-profile-name>
                        </div>
                    </div>
                </div>
                <div class="column is-narrow">
                    <ok-community-profile-action-buttons :community="community"></ok-community-profile-action-buttons>
                </div>
            </div>
        </div>
        <div :class="{'ok-has-border-bottom-primary-highlight': community.description}">
            <p class="menu-label ok-has-text-primary-invert-80 has-padding-left-20 has-padding-right-20 has-padding-top-20 is-marginless">
                {{$t('global.keywords.about')}}
            </p>
            <div v-if="community.description" class="content ok-has-text-primary-invert has-padding-top-10 has-padding-right-20 has-padding-left-20 has-padding-bottom-20">
                <ok-smart-text :text="community.description"></ok-smart-text>
            </div>
        </div>
        <div v-for="field in filteredGroupTypesFields" :class="{'ok-has-border-bottom-primary-highlight': community[field.key]}" :key="field.key">
            <p class="menu-label ok-has-text-primary-invert-80 has-padding-left-20 has-padding-right-20 has-padding-top-20 is-marginless">
                {{ $t(`forms.create_community.details.${field.key}.label`) }}
            </p>
            <div class="content ok-has-text-primary-invert has-padding-top-10 has-padding-right-20 has-padding-left-20 has-padding-bottom-20">
                <ok-smart-text :text="community[field.key]"></ok-smart-text>
            </div>
        </div>
        <div class="card-content ok-has-border-bottom-primary-highlight">
            <div class="columns is-mobile flex-direction-column is-variable is-3 is-marginless">
                <div class="column">
                    <ok-community-profile-visibility :community="community"></ok-community-profile-visibility>
                </div>
                <div class="column">
                    <ok-community-profile-categories :categories="community.categories ? community.categories : []"></ok-community-profile-categories>
                </div>
            </div>
        </div>
        <div class="card-footer">
            <div class="has-padding-left-10">
                <ok-community-profile-info-buttons :community="community"></ok-community-profile-info-buttons>
            </div>
        </div>
    </div>
</template>


<script lang="ts">
    import { Component, Prop, Vue } from "nuxt-property-decorator"
    import { ICommunity } from "~/models/communities/community/ICommunity";
    import OkSmartText from '~/components/smart-text/OkSmartText.vue';
    import OkCommunityProfileVisibility
        from '~/pages/home/pages/community/components/shared/OkCommunityProfileVisibility.vue';
    import OkCommunityProfileCategories
        from '~/pages/home/pages/community/components/shared/OkCommunityProfileCategories.vue';
    import OkCommunityAvatar from '~/components/avatars/community-avatar/OkCommunityAvatar.vue';
    import OkJoinCommunityButton from '~/components/buttons/OkJoinCommunityButton.vue';
    import { GROUP_TYPES, GROUP_TYPES_FIELDS } from '~/config/groups';
    import OkCommunityProfileActionButtons
        from '~/pages/home/pages/community/components/shared/OkCommunityProfileActionButtons.vue';
    import OkMobileCommunityProfileTitle
        from '~/pages/home/pages/community/components/mobile-community-profile/components/mobile-community-profile-card/components/OkMobileCommunityProfileTitle.vue';
    import OkMobileCommunityProfileName
        from '~/pages/home/pages/community/components/mobile-community-profile/components/mobile-community-profile-card/components/OkMobileCommunityProfileName.vue';
    import OkCommunityProfileInfoButtons
        from '~/pages/home/pages/community/components/shared/OkCommunityProfileInfoButtons.vue';

    @Component({
        name: "OkDesktopCommunityProfileSidebar",
        components: {
            OkCommunityProfileInfoButtons,
            OkMobileCommunityProfileName,
            OkMobileCommunityProfileTitle,
            OkCommunityProfileActionButtons,
            OkJoinCommunityButton,
            OkCommunityAvatar,
            OkCommunityProfileCategories,
            OkCommunityProfileVisibility,
            OkSmartText,
        },
    })
    export default class OkDesktopCommunityProfileSidebar extends Vue {
        @Prop({
            type: Object,
            required: true
        }) readonly community: ICommunity;

        @Prop({
            type: Boolean,
            required: true
        }) readonly headerVisible: boolean;
        
        filteredGroupTypesFields = [];

        created() {
            if (this.community.groupType) {
                console.log(this.community.groupType);
                const groupTypeConfig = GROUP_TYPES.find(gT => gT.key === this.community.groupType);
                if (groupTypeConfig) {
                    this.filteredGroupTypesFields = GROUP_TYPES_FIELDS.filter(field => {
                        return groupTypeConfig.fields.includes(field.key) && !!this.community[field.key];
                    });
                }
            }
        }
    }
</script>



