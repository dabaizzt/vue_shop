## 需要了解的css样式
 box-sizing: border-box;


 ## 图标库
 使用阿里图标 
 - 需要把file:///Users/bytedance/IdeaProjects/zhiting/vue_shop2/src/assets/fonts 目录粘贴到项目的
src/assets目录下（该目录用于存放静态资源文件）
- 在 main.js中引入字体图标
import './assets/fonts/iconfont.css'

## vue 
- :model 数据绑定
    -  :model="form", 在表单中输入的内容都会自动同步到对象 “form”上
    - 在data() 中定义 form 对象
    - 每一个表单项的文本输入框通过 v-model 属性绑定到 对象 form的具体属性中，比如:form.name

- 表单验证
    - 在防止用户犯错的前提下，尽可能让用户更早地发现并纠正错误。
    - 在 表单中添加 :rules="rules"
    - 在data(){}中添加 rules:{}
    - 通过 prop="name",可以为输入框添加验证规则（注意是加在 el-form-item 中的），规则对应的是data中的rules中的name
        - required: true          表示是一个必填项
        - message: '请输入活动名称' 提示信息
        - trigger: 'blur'         当鼠标失去焦点的时候触发
        -  { required: true, message: '请输入活动名称', trigger: 'blur' }
        - { min: 3, max: 5, message: '长度在 3 到 5 个字符', trigger: 'blur' }
        - 最小长度是 3  最大长度是5 

-表单重置
    - 通过 ref 拿到表单的对象, 给el-form添加 ref="loginFormRef"
    - 重置按钮添加 @click="resetLoginForm"
    - resetLoginForm方法中实现 this.$refs.loginFormRef.resetFields()
    - resetFields : 对整个表单进行重置，将所有字段值重置为初始值并移除校验结果	

- 表单校验
    - validate : 对整个表单进行校验的方法，参数为一个回调函数。该回调函数会在校验结束后被调用，并传入两个参数：是否校验成功和未通过校验的字段。若不传入回调函数，则会返回一个 promise	

- 动态绑定
   - :key :index
   - 通过在添加: 改为动态绑定
   - 比如 <i :class="iconsObj[item.id]"></i> 因为是要从data()中去读iconsObj字典，所以class要改为动态绑定的 :class

- unique-opened	 是否只保持一个子菜单的展开 

- 侧边栏折叠
    - :collapse="isCollapse" :collapse-transition="false"
    - <el-aside :width="isCollapse ? '64px' : '200px'"> 根据isCollapse的值，动态的修改侧边栏的宽度

- 为整个侧边栏开启路由配置
    - :router="true" 也可以直接简写为 router

- default-active	当前激活菜单的 index	string
    -  <el-aside  default-active="/users">

- 保存导航链接的状态
    - item 增加click事件，<el-menu-item  @click="saveNavState('/'+subItem.path)">
    - 增加函数 saveNavState
    ```
    saveNavState(activePath) {
      window.sessionStorage.setItem('activePath', activePath)
      this.activePath = activePath
    }
    ```
    - data() 中定义变量 activePath:''
    - 修改 <el-menu :default-active="activePath">


- 全局样式表中
```
.el-breadcrumb {
    margin-bottom: 15px;
}

.el-card {
    box-shadow: 0 1px 1px rgba(0, 0, 0, 0.15) !important;
}
```

- el-row
    - :span  指定列的宽度
    - 总共一行时 24 个格子
    - :span="6" 表示占 6 个格子
    - gutter 属性来指定每一栏之间的间隔，默认间隔为 0

- async&await用法
    - async 表示函数里有异步操作
    - await 表示紧跟在后面的表达式需要等待结果

- 表格加边框和斑马纹
    - <el-table :data="userlist" border stripe>
    - border 边框
    - stripe 斑马纹
    - 全局样式增加一条：
    ```
    .el-table {
        margin-top: 15px;
    }
    ```
- 插槽
···
 <template slot-scope="scope">            
    {{scope.row}}  //表示拿到这一行的数据
</template>
···

- 分页的实现
    - getUserList() 发起的get 请求将封装了分页信息的this.queryInfo作为参数 请求到了后端
    - 添加分页栏模块
    ```
    <el-pagination
    @size-change="handleSizeChange"
    @current-change="handleCurrentChange"
    :current-page="queryInfo.pagenum"
    :page-sizes="[1, 2, 5, 10]"
    :page-size="queryInfo.pagesize"
    layout="total, sizes, prev, pager, next, jumper"
    :total="total">
</el-pagination>
    ```
    - 实现函数 handleSizeChange 和 handleCurrentChange

- 搜索功能
    - 搜索框绑定数据 v-model="queryInfo.query"，el-input placeholder="请输入内容" v-model="queryInfo.query">
    - 搜索按钮添加点击事件，@click="getUserList()

- 清空文本框
    - 文本框添加属性 clearable
    - 添加 @clear 事件

- 对话框
    - :visible.sync ： 对话框的显示和隐藏

- 邮箱添加正则表达式校验
```
  -  // 验证邮箱的规则
    var checkEmail = (rule, value, callback) => {
      // 验证邮箱的正则表达式
      const regEmail = /^([a-zA-Z0-9_-])+@([a-zA-Z0-9_-])+(\.[a-zA-Z0-9_-])+/
      if (regEmail.test(value)) {
        // 合法的邮箱
        return callback()
      }
      callback(new Error('请输入合法的邮箱'))
    }
```
 - 添加规则
 { validator: checkEmail, trigger: 'blur'}

- 手机添加正则表达式校验
```
    // 验证手机的规则
    var checkMobile = (rule, value, callback) => {
      // 验证手机号的正则表达式
      const regMobile = /^(0|86|17951)?(13[0-9]|15[012356789]|17[678]|18[0-9]|14[57])[0-9]{8}$/
      if (regMobile.test(value)) {
        return callback()
      }
      callback(new Error('请输入合法的手机号'))
    }
```
- 添加规则
{ validator: checkMobile, trigger: 'blur'}

- 重置对话框中的表单
    - 对话框添加 @close="addDialogClosed"
    - 实现方法 addDialogClosed 
    ```
    // 监听添加用户对话框的关闭事件
    addDialogClosed() {
      this.$refs.addFormRef.resetFields()
    }
    ```

# git技能
- 从现有分支中 checkout出一个新的分支： git checkout -b rights
- 将新创建的分支推送到git上面，git push -u origin rights

