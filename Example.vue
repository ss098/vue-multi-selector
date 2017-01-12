<template>
    <div>
        <selector :source="selector_data" @click-item="clickItem" @click-checked="clickChecked"></selector>
        <p>最后一次点击：{{ item.name }}</p>
        你勾选了：
        <ul v-if="checkedList && checkedList.length > 0">
            <li v-for="i in checkedList">{{ i }}</li>
        </ul>
    </div>
</template>
<script>
    import Selector from './src/Selector.vue'
    export default {
        components: {
            Selector
        },
        data: () => {
            return {
                selector_data: {
                    // 动态构造属性
                    items: [
                        {name: 'PHP', children: {
                            items: [
                                {name: 'Laravel', children: {
                                    items: [
                                        {name: 'Blade'},
                                        {name: 'Elixir'},
                                        {name: 'Migrations'},
                                        {name: 'Seeder'},
                                        {name: 'Eloquent ORM'}
                                    ]
                                }},
                                {name: 'Zend'},
                                {name: 'Cake'}
                            ]
                        }},
                        {name: 'JavaScript', children: {
                            items: [
                                {name: 'Vue.js'},
                                {name: 'React.js'},
                                {name: 'Angular.js'},
                                {name: 'Bootstrap'},
                                {name: 'Amaze UI'}
                            ]
                        }},
                        {name: 'Python', children: {
                            items: [
                                {name: 'Django'},
                                {name: 'Flask'},
                                {name: 'Tornado'}
                            ]
                        }}
                    ]
                },
                item: {},
                checkedList: {}
            }
        },
        methods: {
            clickItem: function (checkedList, item) {
                this.checkedList = checkedList
                this.item = item
            },
            clickChecked: function (checkedList, item) {
                this.checkedList = checkedList
                this.item = item
            }
        }
    }
</script>