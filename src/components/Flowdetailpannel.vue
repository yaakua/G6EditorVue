<!-- 流程图 属性配置组件 -->
<template>
    <div class="detailpannel" id="detailpannel">
        <div data-status="node-selected" class="pannel">
            <div class="pannel-title">节点属性栏</div>
            <div class="block-container">
                <label>id</label><input v-model="current.id" />
                <label>属性名</label><input v-model="current.name" />
                <button @click="saveProp">保存</button>
            </div>
        </div>
        <div data-status="edge-selected" class="pannel"> <!-- 这个地方一定要用class名字为pannel -->
            <div class="pannel-title">边</div>
            <div class="block-container">
                边
            </div>
        </div>
        <div data-status="group-selected" class="pannel">
            <div class="pannel-title">组</div>
            <div class="block-container">
                组
            </div>
        </div>
        <div data-status="canvas-selected" class="pannel">
            <div class="pannel-title">画布</div>
            <div class="block-container">
                画布
            </div>
        </div>
        <div data-status="multi-selected" class="pannel">
            <div class="pannel-title">多选</div>
            <div class="block-container">
                多选
            </div>
        </div>
    </div>
</template>
<script>
    import G6Editor from '@antv/g6-editor';

    export default {
        data() {
            return {
                //当前节点属性
                current: {},
                //保存所有节点属性数据，采用id为key值
                nodeProps: [],
            }
        },
        props: ['editor'],
        mounted() {
            const detailpannel = new G6Editor.Detailpannel({
                container: 'detailpannel',
            });
            this.editor.add(detailpannel)
            setTimeout(() => {
                const graph = this.editor.getCurrentPage()
                graph.on('node:click', ev => {
                    console.log("发生节点点击事件", ev)

                    let shape = ev.shape
                    let id = shape.id
                    let node = this.nodeProps[id] || null
                    if (node === null) {
                        node = shape
                        this.nodeProps[id] = node
                    }
                    this.current = node
                });
            }, 2000)
        },
        methods:{
            saveProp(){
                this.nodeProps[this.current.id]=this.current
            },
            //供父类调用，获取当前所有节点的数据
            getNodeProps(){
                return this.nodeProps
            }
        }
    }
</script>
<style>
    .detailpannel {
        height: 100%;
        position: absolute;
        right: 0px;
        z-index: 2;
        background: #F7F9FB;
        width: 200px;
        border-left: 1px solid #E6E9ED;
    }

    .detailpannel .pannel {
        display: none
    }

    .detailpannel .block-container {
        padding: 16px 8px;
    }

    .pannel-title {
        height: 32px;
        border-top: 1px solid #DCE3E8;
        border-bottom: 1px solid #DCE3E8;
        background: #EBEEF2;
        color: #000;
        line-height: 28px;
        padding-left: 12px;
    }
</style>