<template>
    <div class="columns is-multiline is-variable is-1 is-mobile">
        <div class="column is-narrow is-paddingless has-padding-right-10 has-padding-top-10"
             v-for="community in communities">
            <ok-dynamic-styles-button @handleButtonClick="communityRedirect" :community="community" :key="community.name"/>
        </div>
    </div>
</template>

<script lang="ts">
import {Component, Prop, Vue} from "nuxt-property-decorator";
import {ICommunity} from "~/models/communities/community/ICommunity";
import OkDynamicStylesButton from "~/components/buttons/OkDynamicStylesButton.vue";


@Component({
    name: "OkUserProfileGroups",
    components: {OkDynamicStylesButton},
})
export default class OkUserProfileGroups extends Vue {

    @Prop({
        type: Array,
        required: true
    }) readonly communities: ICommunity;

    communityRedirect(community) {
        this.$router.push({ path: `/c/${community.name}` })
    }

}

</script>

