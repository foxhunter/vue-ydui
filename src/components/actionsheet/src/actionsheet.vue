<template>
    <div>
        <yd-mask v-model="show" @click.native="close"></yd-mask>
        <div class="yd-actionsheet" :class="show ? 'yd-actionsheet-active' : ''">
            <a v-for="item, key in items" @click.stop="itemClick(item)" href="javascript:;" class="yd-actionsheet-item" :key="key">{{item.label}}</a>
            <a v-if="cancel" @click.stop="close" href="javascript:;" class="yd-actionsheet-action">{{cancel}}</a>
        </div>
    </div>
</template>

<script type="text/babel">
    import Mask from '../../mask/src/mask.vue';

    export default {
        name: 'yd-actionsheet',
        components: {
            'yd-mask': Mask
        },
        data() {
            return {
                show: this.value
            }
        },
        props: {
            value: {
                type: Boolean,
                default: false
            },
            items: {
                type: Array,
                require: true
            },
            cancel: String
        },
        watch: {
            value(val) {
                this.show = val;
            }
        },
        methods: {
            itemClick(item) {
                if(item) {
                    // TODO 参数更名，即将删除
                    if (typeof item.method === 'function') {
                        item.method(item);
                        console.warn('From VUE-YDUI: The parameter "method" is destroyed, please use "callback".');
                    }
                    typeof item.callback === 'function' && item.callback(item);
                    !item.stay && this.close();
                }
            },
            close() {
                this.$emit('input', false);
            }
        },
        destroyed() {
            this.close();
        }
    }
</script>

<style lang="less">
    @import "../../../styles/components/actionsheet.less";
</style>
