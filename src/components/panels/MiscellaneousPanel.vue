<style>
    .icon-rotate {
        animation-name: spin;
        animation-duration: 1000ms;
        animation-iteration-count: infinite;
        animation-timing-function: linear;
    }

    @keyframes spin {
        from {
            transform: rotate(0deg);
        } to {
              transform: rotate(-360deg);
          }
    }
</style>

<template>
    <div>
        <v-card class="mt-6" v-if="['printing', 'paused'].includes(printer_state)">
            <v-toolbar flat dense >
                <v-toolbar-title>
                    <span class="subheading"><v-icon class="mdi mdi-printer-3d" left></v-icon>Print Settings</span>
                </v-toolbar-title>
            </v-toolbar>
            <tool-slider label="Speed factor" :target="speed_factor" :max="200" :multi="100" :step="5" command="M220" attribute-name="S" ></tool-slider>
            <v-divider></v-divider>
            <tool-slider label="Extrusion factor" :target="extrude_factor" :max="200" :multi="100" :step="1" command="M221" attribute-name="S" ></tool-slider>
        </v-card>
        <v-card class="mt-6" v-if="this['printer/getMiscellaneous'].length">
            <v-toolbar flat dense >
                <v-toolbar-title>
                    <span class="subheading"><v-icon left>mdi-dip-switch</v-icon>Miscellaneous</span>
                </v-toolbar-title>
            </v-toolbar>
            <div v-for="(object, index) of this['printer/getMiscellaneous']" v-bind:key="index">
                <v-divider v-if="index"></v-divider>
                <miscellaneous-slider :name="object.name" :type="object.type" :target="object.power" :controllable="object.controllable" :pwm="object.pwm" :multi="parseInt(object.scale)"></miscellaneous-slider>
            </div>
        </v-card>
    </div>
</template>

<script>
    import { mapState, mapGetters } from 'vuex'
    import ToolSlider from '../../inputs/ToolSlider'
    import MiscellaneousSlider from "@/inputs/MiscellaneousSlider";

    export default {
        components: {
            MiscellaneousSlider,
            ToolSlider
        },
        data () {
            return {

            }
        },
        computed: {
            ...mapState({
                extrude_factor: state => state.printer.gcode_move.extrude_factor,
                speed_factor: state => state.printer.gcode_move.speed_factor,
                printer_state: state => state.printer.print_stats.state,
            }),
            ...mapGetters([
                'printer/getMiscellaneous',
            ]),
        },
    }
</script>
