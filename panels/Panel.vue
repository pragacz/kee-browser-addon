<template>
    <v-app id="inspire">
        <v-main>
            <v-container fluid>
                <v-menu top offset-y small>
                    <template v-slot:activator="{ on }">
                        <v-btn icon small v-on="on">
                            <v-icon>mdi-menu</v-icon>
                        </v-btn>
                    </template>

                    <v-list>
                        <v-list-item @click="showPasswordGenerator = true">
                            <v-list-item-title class="mr-4 text-right body-2">
                                {{ $i18n("Menu_Button_copyNewPasswordToClipboard_label") }}
                            </v-list-item-title>
                            <v-icon size="20px">
                                mdi-flash
                            </v-icon>
                        </v-list-item>
                    </v-list>
                </v-menu>
            </v-container>
        </v-main>

        <PasswordGenerator
            v-if="showPasswordGenerator"
            :standalone="true"
            @dialog-closed="passwordGeneratorClosed"
            @copy-to-clipboard="copyToClipboard"
        />
    </v-app>
</template>

<script lang="ts">
import { Component } from "vue";
import { mapState, mapActions, mapGetters, mapMutations } from "vuex";
import { names as actionNames } from "../store/action-names";
import { SessionType } from "../common/SessionType";
import { KeeState } from "../store/KeeState";
import PasswordGenerator from "../common/components/PasswordGenerator.vue";
import { Port } from "../common/port";
import { Action } from "../common/Action";
import { KeeLog } from "../common/Logger";
import { KeeVue } from "../common/KeeVue";
import { Entry } from "../common/model/Entry";
import { copyStringToClipboard } from "./copyStringToClipboard";

export default {
    components: {
        PasswordGenerator
    },
    mixins: [Port.mixin],
    data: () => ({
        showPasswordGenerator: false
    }),
    computed: {
        ...mapGetters([
            "showGeneratePasswordLink",
            "connectionStatus",
            "connectionStatusDetail",
            "connected",
            "databaseIsOpen"
        ])
    },
    methods: {
        ...mapActions(actionNames),
        passwordGeneratorClosed: function (this: any) {
            this.showPasswordGenerator = false;
        },
        copyToClipboard: async function (this: any, payload) {
            if (payload?.value) await copyStringToClipboard(payload.value);
        }
    }
};
</script>

<style>
.v-main__wrap {
    overflow-y: scroll;
}

.app_height_medium .v-main__wrap {
    max-height: 466px;
    min-height: 466px;
    height: 466px;
}

.app_height_tall .v-main__wrap {
    max-height: 522px;
    min-height: 522px;
    height: 522px;
}

.pulse-button {
    animation: pulse 0.5s 2 cubic-bezier(0.45, 0.05, 0.55, 0.95) 0.25s reverse;
}

@keyframes pulse {
    0% {
        opacity: 1;
        transform: scale(1);
    }
    25% {
        opacity: 1;
        transform: scale(1.25);
    }
    50% {
        opacity: 1;
        transform: scale(1);
    }
    75% {
        opacity: 1;
        transform: scale(0.8);
    }
    100% {
        opacity: 1;
        transform: scale(1);
    }
}
</style>
