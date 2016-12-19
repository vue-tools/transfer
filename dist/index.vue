<style src="./style.css"></style>
<template src="./template.html"></template>
<script>
    import Checkboxs from 'checkbox/dist/index.vue'
    import Buttons from 'button/dist/index.vue'
    export default {
        props: {
            showSearch: {
                type: Boolean,
                default: true
            },
            showHeader: {
                type: Boolean,
                default: true
            },
            dataSource: {
                type: Array,
                required: true
            },
            dataTarget: {
                type: Array,
                default(){
                    return []
                }
            },
            showField: String
        },
        data(){
            return {
                emptyInfo: '没有选项',
                searchSource: '',
                searchTarget: '',
                targets: this.dataTarget,
                sources: this.dataSource
            }
        },
        computed: {
            filterTargets(){
                return this.targets.filter(item => item[this.showField].indexOf(this.searchTarget) != -1)
            },
            filterSources(){
                return this.sources.filter(item => item[this.showField].indexOf(this.searchSource) != -1)
            },
            allSource: {
                get(){
                    if(this.selectedSources.length == 0)  return false
                    return this.selectedSources.length == this.sources.length
                },
                set(val){
                    this.sources.map(item => item.checked = val)
                }
            },
            allTarget: {
                get(){
                    if(this.selectedTargets.length == 0)  return false
                    return this.selectedTargets.length == this.targets.length
                },
                set(val){
                    this.targets.map(item => item.checked = val)
                }
            },
            selectedTargets(){
                return this.targets.filter(item => item.checked)
            },
            selectedSources(){
                return this.sources.filter(item => item.checked)
            }
        },
        methods: {
            add(){
                let idx, preSelectedSources = Object.assign([], this.selectedSources)
                this.selectedSources.forEach(item => {
                    idx = this.sources.indexOf(item)
                    item.checked = false
                    this.targets.unshift(item)
                    this.sources.splice(idx, 1)
                })
                this.$emit('change', preSelectedSources, this.sources, this.targets)
            },
            remove(){
                let idx, preSelectedTargets = Object.assign([], this.selectedTargets)
                this.selectedTargets.forEach(item => {
                    idx = this.targets.indexOf(item)
                    item.checked = false
                    this.sources.unshift(item)
                    this.targets.splice(idx, 1)
                })
                this.$emit('target-change', preSelectedTargets, this.sources, this.targets)

            }
        },
        components: {
            Checkboxs,
            Buttons
        }
    }
</script>
