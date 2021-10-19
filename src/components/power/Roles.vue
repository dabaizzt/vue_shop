<template>
    <div>
        <!-- 面包屑导航区域 -->
        <el-breadcrumb separator-class="el-icon-arrow-right">
            <el-breadcrumb-item :to="{ path: '/home' }">首页</el-breadcrumb-item>
            <el-breadcrumb-item>权限管理</el-breadcrumb-item>
            <el-breadcrumb-item>角色列表</el-breadcrumb-item>
        </el-breadcrumb>

        <!-- 卡片视图 -->
        <el-card>
            <!-- 添加角色按钮区域 -->
            <el-row>
                <el-col>
                    <el-button type="primary">添加角色</el-button>
                </el-col>
            </el-row>

            <!-- 角色列表区域 -->
            <el-table :data="roleList" border stripe>
                <!-- 展开列 -->
                <el-table-column type="expand">
                    <template slot-scope="scope">
                       <el-row :class="['dbbottom', index1 ===0 ? 'dbtop' : '', 'vcenter']" v-for="(item1, index1) in scope.row.children" :key="item1.id">
                           <!-- 渲染一级权限 -->
                           <el-col :span="5">
                               <el-tag closable @close="removeRightById(scope.row, item1.id)">{{item1.authName}}</el-tag>
                               <i class="el-icon-caret-right"></i>
                           </el-col>
                           <!-- 渲染二级和三级权限 -->
                           <el-col :span="19">
                               <el-row :class="[index2 == 0 ? '':'dbtop', 'vcenter']" v-for="(item2, index2) in item1.children" :key="item2.id">
                                   <el-col :span="6">
                                       <el-tag type="success" closable @close="removeRightById(scope.row, item2.id)">{{item2.authName}}</el-tag>
                                       <i class="el-icon-caret-right"></i>
                                   </el-col>
                                   <el-col :span="18">
                                       <el-tag type="warning" v-for="(item3, index3) in item2.children" :key="item3.id" closable @close="removeRightById(scope.row, item3.id)">
                                           {{item3.authName}}
                                       </el-tag>
                                   </el-col>
                               </el-row>
                           </el-col>
                       </el-row>
                    </template>
                </el-table-column>
                <el-table-column type="index"></el-table-column>
                <el-table-column label="角色名称" prop="roleName"></el-table-column>
                <el-table-column label="角色描述" prop="roleDesc"></el-table-column>
                <el-table-column label="操作" width="300px">
                    <template slot-scope="scope">
                        <el-button size="mini" type="primary" icon="el-icon-edit">编辑</el-button>
                        <el-button size="mini" type="danger" icon="el-icon-delete">删除</el-button>
                        <el-button size="mini" type="warning" icon="el-icon-setting">分配权限</el-button>
                    </template>
                </el-table-column>
            </el-table>
        </el-card>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                // 所有权限列表
                roleList: []
            }
        },
        created() {
            this.getRolesList()
        },
        methods: {
            // 获取所有角色列表
            async getRolesList() {
                const {data: res} = await this.$http.get('roles')

                if(res.meta.status !== 200) {
                    return this.$message.error('获取角色列表失败！')
                }
                
                this.roleList = res.data
                console.log(this.roleList)
            },

            // 根据id删除对应权限
            async removeRightById(role, rightId) {
                // 弹框提示用户是否要删除
                const confirmResult = await
                this.$confirm('此操作将永久删除该权限, 是否继续?', '提示', {
                    confirmButtonText: '确定',
                    cancelButtonText: '取消',
                    type: 'warning'
                }).catch(err => err)

                if(confirmResult !== 'confirm') {
                    return this.$message.info('取消了删除！')
                }

                //  删除权限
                const{data: res} = await this.$http.delete(`roles/${role.id}/rights/${rightId}`)
                // 删除失败
                if(res.meta.status !== 200) {
                    return this.$message.error('删除权限失败！')
                }
                // 删除成功，刷新权限列表
                this.$message.success('删除权限成功！')
                // 不建议调用该函数 会发生页面的完整渲染，导致打开的角色列表都关闭了
                // this.getRolesList()
                // console.log(res.data) //删除接口 返回了当前操作角色的权限
                role.children = res.data
            }
        }
    }
</script>

<style scoped>
.el-tag {
    margin: 7px;
}

.dbtop {
    border-top: 1px solid #eee
}

.dbbottom {
    border-bottom: 1px solid #eee;
}

.vcenter {
    display: flex;
    align-items: center;
}
</style>
