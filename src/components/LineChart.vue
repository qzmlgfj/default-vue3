<template>
    <v-chart class="chart" :option="option" />
</template>

<script>
import { use } from "echarts/core";
import {
    TitleComponent,
    ToolboxComponent,
    TooltipComponent,
    GridComponent,
    DataZoomComponent,
} from "echarts/components";
import { LineChart } from "echarts/charts";
import { UniversalTransition } from "echarts/features";
import { CanvasRenderer } from "echarts/renderers";
import VChart, { UPDATE_OPTIONS_KEY } from "vue-echarts";
import { ref } from "vue";

use([
    TitleComponent,
    ToolboxComponent,
    TooltipComponent,
    GridComponent,
    DataZoomComponent,
    LineChart,
    CanvasRenderer,
    UniversalTransition,
]);

export default {
    components: {
        VChart,
    },
    provide() {
        return {
            [UPDATE_OPTIONS_KEY]: {
                notMerge: false,
            },
        };
    },
    setup() {
        function randomData() {
            now = new Date(+now + oneDay);
            value = value + Math.random() * 21 - 10;
            return {
                name: now.toString(),
                value: [
                    [now.getFullYear(), now.getMonth() + 1, now.getDate()].join(
                        "/"
                    ),
                    Math.round(value),
                ],
            };
        }
        let fakeData = ref([]);
        let now = new Date(1997, 9, 3);
        let oneDay = 24 * 3600 * 1000;
        let value = Math.random() * 1000;
        for (var i = 0; i < 1000; i++) {
            fakeData.value.push(randomData());
        }

        setInterval(() => {
            for (var i = 0; i < 5; i++) {
                fakeData.value.shift();
                fakeData.value.push(randomData());
            }
        }, 1000);

        return {
            fakeData,
        };
    },
    data() {
        return {
            option: {
                tooltip: {
                    trigger: "axis",
                    position: function (pt) {
                        return [pt[0], "10%"];
                    },
                },
                title: {
                    left: "center",
                    text: "Large Ara Chart",
                },
                toolbox: {
                    feature: {
                        dataZoom: {
                            yAxisIndex: "none",
                        },
                        restore: {},
                        saveAsImage: {},
                    },
                },
                xAxis: {
                    type: "time",
                    boundaryGap: false,
                },
                yAxis: {
                    type: "value",
                    boundaryGap: [0, "100%"],
                },
                dataZoom: [
                    {
                        type: "inside",
                        start: 0,
                        end: 20,
                    },
                    {
                        start: 0,
                        end: 20,
                    },
                ],
                series: [
                    {
                        name: "Fake Data",
                        type: "line",
                        smooth: true,
                        symbol: "none",
                        areaStyle: {},
                        data: this.fakeData,
                    },
                ],
            },
        };
    },
};
</script>
