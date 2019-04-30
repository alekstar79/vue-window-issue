<template>
    <component :is="dynamicStyle">
        <window-type
            :left.sync="left"
            :top.sync="top"
            :title="title"
            :zGroup="1"
            ref="window"
        >
            <p>Parameters:</p>

            <fieldset>
                <legend>&alpha;</legend>
                <label for="alpha"></label>
                <input type="range" id="alpha" />
            </fieldset>
            <fieldset>
                <legend>&beta;</legend>
                <label for="betha"></label>
                <input type="range" id="betha" />
            </fieldset>
        </window-type>
    </component>
</template>

<script>
    import { StyleFactory, WindowType } from '@hscmap/vue-window'
    // import colors from 'plugins/theme'

    import { mapState } from 'vuex'

    export default {
        components: {
            WindowType
        },
        props: {
            title: {
                required: true,
                type: String
            }
        },
        data() {
            return {
                dynamicStyle: null,
                left: null,
                top: null
            }
        },
        /* computed: {
            ...mapState('app', ['color'])
        }, */
        beforeDestroy()
        {
            // and this can be simply remove for tests
            this.$ls.set('position', {
                left: this.left,
                top: this.top
            })
        },
        beforeMount()
        {
            // this.$ls - local storage, you can simply hardcode the values for tests
            const { left, top } = this.$ls.get('position')

            this.left = left
            this.top = top

            this.dynamicStyle = StyleFactory({
                window: {
                    // left: `${left}px`,
                    // top: `${top}px`,
                    color: 'rgb(0,0,0)',
                    boxShadow: 'rgba(0,0,0,.5) 0px 2pt 4pt',
                    backgroundColor: 'rgba(239,239,239,.95)',
                    zIndex: 1
                },
                titlebar: {
                    backgroundColor: '#00d3ee'/* colors[this.color] */
                },
                content: {
                    padding: '9px'
                }
            })
        },
        mounted()
        {
            /* const { left, top } = this.$ls.get('position')

            this.$nextTick().then(() => {
                this.$refs.window.left = left || 300
                this.$refs.window.top = top || 200
            }) */
        }
    }
</script>
