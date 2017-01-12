<template>
    <div class="selector">
        <ul v-for="(line, deep) in menu" class="selector-item">
            <li v-for="(item, index) in line.items"
                @click="displayChindren(line, item, index)"
                v-bind:class="{
                    'active': index == deepIndex[deep],
                    'children': item.children
                }">
                {{ item.name }}

                <input v-if="!item.children && deep + 1 == menu.length" v-model="checkedList[deep][index]" type="checkbox" @click="setCheck($event, deep, index)">
            </li>
        </ul>
    </div>
</template>
<style scoped>
    .selector {
        display: inline-block;
        background: #fff;
        white-space: nowrap;
        position: relative;
        z-index: 1;
        border: 1px solid #d9d9d9;
        overflow-x: 'auto'
    }
    .selector-item {
        display: inline-block;
        vertical-align: top;
        list-style: none;
        margin: 0;
        padding: 0;
        overflow: auto;
        cursor: pointer;
    }
    .selector .selector-item li {
        padding: 4px;
        position: relative;
        line-height: 1.8;
        transition: border-right 200ms;
        min-width: 160px;
    }
    .selector .selector-item li.children {
        color: #3354AA;
    }
    .selector .selector-item li.active,
    .selector .selector-item li:hover {
        color: white;
        background-color: #20A0FF;
    }
    input[type="checkbox"] {
        margin-top: 8px;
        float: right;
    }
</style>
<script>
    export default {
        data: function () {
            return {
                menu: [],
                deepIndex: [],
                checkedList: []
            }
        },
        props: {
            source: {
                type: Object,
                required: true,
                default: function () {
                    return {}
                }
            }
        },
        methods: {
            // 展示它的子层级
            displayChindren: function (line, item, index) {
                let deep = line.deep
                let deepIndex = this.deepIndex
                let checkedList = this.checkedList

                this.$set(deepIndex, deep, index)
                this.deleteDeep(deep)

                if (item.children) {
                    this.$set(item, 'deep', deep + 1)
                    this.$set(deepIndex, deep + 1, void 0)
                    this.$set(checkedList, deep + 1, {})
                    this.pushMenu(item.children, item.deep)
                }

                this.$emit('click-item', this.getCheckedList(deep), item)
            },

            // 将 item 添加到 menu 并为之生成 deep
            pushMenu: function (item, deep) {
                let menu = this.menu
                item.deep = deep
                menu.push(item)
            },

            // 从 Menu 中删掉索引大于 Deep 的
            deleteDeep: function (deep) {
                let menu = this.menu
                for (let i = menu.length - 1; i > deep; i--) {
                    menu.splice(i, 1)
                }
            },

            // 将当前位置推入到已选列表中
            setCheck: function (event, deep, index) {
                let checked = event.target.checked
                let checkedList = this.checkedList
                let menu = this.menu

                if (!checkedList[deep]) {
                    checkedList[deep] = {}
                }

                if (checked) {
                    checkedList[deep][index] = checked
                } else {
                    delete checkedList[deep][index]
                }

                this.$emit('change-checked', this.getCheckedList(deep), menu[deep].items[index])
            },
            getCheckedList: function (deep) {
                let menu = this.menu
                let checkedList = this.checkedList

                let list = []
                for (let i in checkedList[deep]) {
                    list.push(this.menu[deep].items[i])
                }
                return list
            }
        },
        mounted: function () {
            let source = this.source
            this.pushMenu(source, 0)
        }
    }
</script>