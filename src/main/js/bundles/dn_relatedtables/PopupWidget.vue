<!--

    Copyright (C) 2020 con terra GmbH (info@conterra.de)

    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

            http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.

-->
<template>
    <div>
        <v-progress-linear
            v-if="loading"
            :indeterminate="true"
            class="pa-0 ma-0"/>
        <v-tabs
            v-model="activeTab"
            show-arrows
            slider-color="primary">
            <v-tab ripple>
                {{ i18n.attributes }}
            </v-tab>
            <v-tab
                v-for="relatedRecordsTab in relatedRecordsTabs"
                :key="relatedRecordsTab.id"
                ripple
            >
                {{ relatedRecordsTab.title }}
            </v-tab>
            <v-tab-item>
                <v-data-table
                    :headers="headers"
                    :items="items"
                    hide-actions>
                    <template v-slot:items="props">
                        <td class>{{ props.item.alias }}</td>
                        <td
                            v-if="isUrl(props.item.value)"
                            class
                        >
                            <a
                                :href="props.item.value"
                                target="_blank"
                            >
                                {{ props.item.value }}
                            </a>
                        </td>
                        <td
                            v-else
                            class
                        >
                            {{ props.item.value }}
                        </td>
                    </template>
                </v-data-table>
            </v-tab-item>
            <v-tab-item
                v-for="relatedRecordsTab in relatedRecordsTabs"
                :key="relatedRecordsTab.id">
                <v-autocomplete
                    v-model="relatedRecordsTab.active"
                    :items="relatedRecordsTab.relatedRecords"
                    :label="i18n.relatedRecord"
                    :menu-props="{ contentClass: 'relatedTableMenu' }"
                    class="mt-3 pb-2"
                    return-object
                    hide-details
                    item-text="title"
                ></v-autocomplete>
                <v-data-table
                    :headers="headers"
                    :items="relatedRecordsTab.active ? relatedRecordsTab.active.items : []"
                    hide-actions>
                    <template v-slot:items="props">
                        <td>{{ props.item.alias }}</td>
                        <td v-if="isUrl(props.item.value)">
                            <a
                                :href="props.item.value"
                                target="_blank"
                            >
                                {{ props.item.value }}
                            </a>
                        </td>
                        <td v-else>
                            {{ props.item.value }}
                        </td>
                    </template>
                </v-data-table>
            </v-tab-item>
        </v-tabs>
    </div>
</template>
<script>
    import Bindable from "apprt-vue/mixins/Bindable";

    export default {
        mixins: [Bindable],
        data() {
            return {
                title: "",
                loading: true,
                items: [],
                headers: [],
                relatedRecordsTabs: [],
                activeTab: 0
            };
        },
        methods: {
            isUrl: function (string) {
                return (
                    new RegExp(
                        "^(http:\\/\\/www\\.|https:\\/\\/www\\.|http:\\/\\/|https:\\/\\/)?[a-z0-9]+([\\-\\.]{1}[a-z0-9]+)*\\.[a-z]{2,5}(:[0-9]{1,5})?(\\/.*)?$"
                    )
                ).test(string);
            }
        }
    };
</script>
