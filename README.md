# vue-paginate

> 分页插件

## 运行项目
        先安装依赖
        npm install 或者 cnpm install
        运行项目
        npm run dev



> 以上是github上的代码的使用和运行，如果你没有兴趣去看源码的话，那么以下的教程能够让你直接使用该插件

## 安装
        npm install cjhcj-paginate --save-dev
## 使用
        <Paginate @getPage='getPage' :pageCss='pageCss' :pageCount='pageCount' :pageSize='pageSize'> 

        </Paginate>
## 参数说明

### pageCss  设置分页样式
        pageCss    用于设置分页的样式，接受两个值 active 和 pages
        active 用于设置被选中的页码的样式，pages用于设置普通页码的样式
        栗子：
        pageCss: {
            pages: {
                background: '#999',
                color: 'black'
            },
            active: {
                background: 'green',
                color: 'white'
            },
        }
### pageCount pageSize
        pageCount   代表总数
        pageSize    代表一页的数量
### getPage   
        getPage(val) {
            console.log(val);
        }
        通过以上的函数能够得到选中的页码
        