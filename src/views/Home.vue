<template>
    <div class="home-container">
        <div class="home-content">
            <el-button type="primary" plain @click="oneClickDeploy" style="margin-right:10px">一键部署</el-button>
            <el-upload class="upload-demo" action="https://jsonplaceholder.typicode.com/posts/"
                :on-preview="handlePreview" :on-remove="handleRemove" :before-remove="beforeRemove" multiple :limit="3"
                :on-exceed="handleExceed" :file-list="fileList">
                <el-button type="primary" plain style="margin-right:10px">上传配置文件</el-button>
            </el-upload>
            <el-button type="primary" plain @click="open('添加网络')">添加网络</el-button>
            <el-button type="primary" plain @click="open('添加组织')">添加组织</el-button>
        </div>
        <el-table :data="tableData" border style="width: 100%">
            <el-table-column fixed prop="id" label="网络实例配置信息ID" min-width="150">
            </el-table-column>
            <el-table-column prop="name" label="组织简称" min-width="120">
            </el-table-column>
            <el-table-column prop="lastdate" label="更新时间" min-width="120">
            </el-table-column>
            <el-table-column prop="createdate" label="创建时间" min-width="120">
            </el-table-column>
            <el-table-column label="操作" min-width="300">
                <!-- slot-scope="scope" -->
                <template slot-scope="scope">
                    <el-button type="text" size="small" @click="openModel(scope.row, scope.$index, 2)">
                        编辑
                    </el-button>
                    <el-button @click="openModel(scope.row, scope.$index, 1)" type="text" size="small">查看</el-button>
                    <el-button type="text" @click.native.prevent="deleteRow(scope.$index, tableData)" size="small">删除
                    </el-button>
                    <el-button type="text" size="small">应用网络</el-button>
                    <el-button type="text" size="small">加入网络</el-button>
                </template>
            </el-table-column>
        </el-table>
        <!--对话框 -->
        <el-dialog :title="title" :visible.sync="dialogFormVisible">
            <el-form :model="modData">
                <el-form-item label="网络实例配置信息ID" :label-width="formLabelWidth">
                    <el-input v-model="modData.id" autocomplete="off" :readonly="readOnly"></el-input>
                </el-form-item>
                <el-form-item label="组织简称" :label-width="formLabelWidth">
                    <el-input v-model="modData.name" autocomplete="off" :readonly="readOnly"></el-input>
                </el-form-item>
                <el-form-item label="更新时间" :label-width="formLabelWidth">
                    <el-input v-model="modData.lastdate" autocomplete="off" :readonly="readOnly"></el-input>
                </el-form-item>
                <el-form-item label="创建时间" :label-width="formLabelWidth">
                    <el-input v-model="modData.createdate" autocomplete="off" :readonly="readOnly"></el-input>
                </el-form-item>
            </el-form>
            <div slot="footer" class="dialog-footer" v-show="isShow">
                <el-button @click="dialogFormVisible = false">取 消</el-button>
                <el-button type="primary" @click="que">确 定</el-button>
            </div>
        </el-dialog>
    </div>
</template>

<script>
export default {
    name: 'home',
    data() {
        return {
            title: '查看数据',
            readOnly: false,
            isShow: true,
            tableData: [{
                id: '14539299209030',
                name: '大中华',
                lastdate: '2022-06-22',
                createdate: '2016-05-04',
            }, {
                id: '14539299819030',
                name: '大中华',
                lastdate: '2022-06-27',
                createdate: '2016-05-04',
            }, {
                id: '14537299819030',
                name: '大中华',
                lastdate: '2022-07-02',
                createdate: '2016-05-01',
            }, {
                id: '14537299819030',
                name: '大中华',
                lastdate: '2022-06-28',
                createdate: '2016-05-03',
            }],
            modData: [],
            dialogFormVisible: false,
            formLabelWidth: '140px',
            currentIndex: 0,
        }
    },
    created() {
        this.open = this._.debounce(this.click, 1000, { leading: true, trailing: false })
    },
    unmounted() {
        this.open.cancel()
    },
    methods: {
        // 一键部署
        oneClickDeploy() {
            this.$alert('是否部署默认网络结构', '一键部署提示', {
                confirmButtonText: '确定',
                center: true,
                type: 'warning',
                callback: () => {
                    this.$message({
                        type: 'success',
                        message: '一键部署成功',
                    })
                },
            })
        },
        handleClick(row) {
            console.log(row)
        },
        click(e) {
            this.$message.success(e)
        },
        // 删除按钮
        deleteRow(index, rows) {
            this.$confirm('此操作将永久删除该行数据, 是否继续?', '提示', {
                confirmButtonText: '确定',
                cancelButtonText: '取消',
                type: 'warning',
            }).then(() => {
                rows.splice(index, 1)
                this.$message({
                    type: 'success',
                    message: '删除成功!',
                })
            }).catch(() => {
                this.$message({
                    type: 'info',
                    message: '已取消删除',
                })
            })
        },
        openModel(row, index, flag) {
            this.dialogFormVisible = true
            this.modData = row
            this.currentIndex = index
            this.title = flag == 1 ? '查看数据' : '修改数据'
            this.readOnly = flag == 1
            this.isShow = !(flag == 1)
        },
        // 确认修改
        que() {
            this.tableData.splice(this.currentIndex, 1, this.modData)
            this.dialogFormVisible = false
        },
    },
}
</script>

<style scoped>
.home-container {
    padding: 10px;
    padding-top: 5px;
}

.home-content {
    display: flex;
    padding: 10px;
    border-radius: 5px;
    background: #fff;
    position: relative;


}
</style>
<style>
.home-container .el-upload-list {
    position: absolute;
    right: 20px;
    top: 0;
}

.home-container .el-message-box .el-message-box__content {
    line-height: 60px;
    text-align: center;
}
</style>