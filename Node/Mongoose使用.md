## Mongoose的使用
```javaScript
const mongoose = require('mongoose')

// 本地数据库地址
const url = 'mongodb://localhost:27017'
// 指定数据库名字
const dbName = 'admin'

// 连接数据库
mongoose.connect(`${url}/${dbName}`, {
    useNewUrlParser: true,
    useUnifiedTopology: true
})

// 数据库连接成功
mongoose.connection.on('connected', () => {
    console.log('数据库连接成功');
})

// 断开连接
mongoose.connection.on('disconnected', (err) => {

})

// 模型类
var Schema = mongoose.Schema

// 创建数据模型
/**
 * 可用属性：
 * required: 布尔值或函数 如果值为真，为此属性添加 required 验证器
   default: 任何值或函数 设置此路径默认值。如果是函数，函数返回值为默认值
   select: 布尔值 指定 query 的默认 projections
   validate: 函数 adds a validator function for this property
   get: 函数 使用 Object.defineProperty() 定义自定义 getter
   set: 函数 使用 Object.defineProperty() 定义自定义 setter
   alias: 字符串 仅mongoose >= 4.10.0。 为该字段路径定义虚拟值 gets/sets
 */
var user = new Schema({
    username: {
        type: String,
        required: true,
        unique: true
    },
    password: {
        type: String,
        required: true,
    },
    age: Number,
    city: String,
    gender: {
        type: Number,
        default: 0
    }
}, { timestamps: true })

// 创建一个model
// Models 是从 Schema 编译来的构造函数。 它们的实例就代表着可以从数据库保存和读取的 documents。 从数据库创建和读取 document 的所有操作都是通过 model 进行的。
// 第一个参数是跟 model 对应的集合（ collection ）名字的 单数 形式。 
// Mongoose 会自动找到名称是 model 名字 复数 形式的 collection 。 
// 对于上例，User 这个 model 就对应数据库中 users 这个 collection。.model() 这个函数是对 schema 做了拷贝（生成了 model）。 你要确保在调用 .model() 之前把所有需要的东西都加进 schema 里了！
var User = mongoose.model('user', user)

// 数据保存
async function save() {
    var user = new User()
    user.username = 'xiangda'
    user.password = 'abc'
    user.age = 20
    user.city = 'shenzhen'
    user.gender = 0
    try {
        const res = await user.save()
        if (res) {
            console.log('数据保存成功');
            console.log(res);
        }
    } catch(err) {
        console.log('数据保存失败');
        console.log(err);
    }
}

// 数据查找
async function find() {
    var erya = await User.findOne({ username: 'erya1', password: '123' })
    if (erya) {
        console.log(erya);
    } else {
        console.log('数据查询失败');
    }
}

module.exports = {
    mongoose,
    User
}
```
