## 需要了解的css样式
 box-sizing: border-box;

## 环境配置
- 安装 axios 到开发依赖中
    - npm i axios --save-dev


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

- 表单重置
    - 通过 ref 拿到表单的对象, 给el-form添加 ref="loginFormRef"
    - 重置按钮添加 @click="resetLoginForm"
    - resetLoginForm方法中实现 this.$refs.loginFormRef.resetFields()
    - resetFields : 对整个表单进行重置，将所有字段值重置为初始值并移除校验结果	

- 表单校验
    - validate : 对整个表单进行校验的方法，参数为一个回调函数。该回调函数会在校验结束后被调用，并传入两个参数：是否校验成功和未通过校验的字段。若不传入回调函数，则会返回一个 promise	

- 弹框提示
    - this.$message.error("登录失败!");
    - this.$message.success("登录成功");


## admin 123456

##  {data: res}  :将data属性从response中解构复制出来

## 格式化文档
- 右键：format document
- 修改格式化的规则
    - 创建文件 .prettierrc

