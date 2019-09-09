<template>
    <div>
        <Cascader :data="data" v-model="value1"></Cascader>
    </div>
</template>
<script>
    import axios from 'axios'
    export default {
        data() {
            return {
                value1: [],
                rowAreaList: [],
                data: []
            }
        },
        methods: {
            proJSON(oldArr, parentId) {
                var newArr = [];
                var self = this;
                oldArr.map(function (item) {
                    if (item.parentId == parentId) {
                        var obj = {
                            id: item.id,
                            value: item.areaCode,
                            label: item.areaName
                        }
                        var children = self.proJSON(oldArr, item.id);
                        if (children.length > 0) {
                            obj.children = children
                        }
                        newArr.push(obj)
                    }

                })
                return newArr;
            },
            getData () {
                axios.get('data.json')
                    .then(res => {
                        this.rowAreaList = res.data.data
                        this.data = this.proJSON(res.data.data, 0)
                    })
            }
        },
        mounted() {
          this.getData()
        }
    }
</script>
