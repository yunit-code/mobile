# 内容列表

这里用来描述组件的具体作用说明，描述内容建议简明扼要。

## 组件类名（className）

IContentList

## 组件类 ID（classId）

idm.component.mobile.icontentlist

## 组件开发语言（comLangue）

vue

## 组件类型（comType）

common

## 所在代码包版本

basics@1.0.0

## 组件属性

### 唯一标识【ctrlId】

### 基本属性

#### 文本内容【fontContent】

#### title【htmlTitle】

#### 默认状态【defaultStatus】

### 样式设置

#### 内外边距【box】

#### 宽高

##### 宽【width】

##### 高【height】

#### 背景设置

##### 背景色【bgColor】

##### 背景图片【bgImgUrl】

##### 横向偏移【positionX】

##### 纵向偏移【positionY】

##### 背景大小【bgSize】

##### 宽度【bgSizeWidth】

##### 高度【bgSizeHeight】

##### 平铺模式【bgRepeat】

##### 背景模式【bgAttachment】

#### 边框【border】

#### 文字【font】

### 高级

#### 点击时动作【clickFunction】

#### 动态内容【dataSourceType】

#### 接口地址【customInterfaceUrl】

#### 结果集名【dataName】

#### 显示字段【dataFiled】

#### 自定义函数【customFunction】

##### 数据源接受参数

```js
// 如果有自定义参数函数
let params = {}
if (this.propData.paramsFunc && this.propData.paramsFunc.length > 0) {
    const funcName = this.propData.paramsFunc[0].name
    params =
        window[funcName].call(this, {
            routerParams: IDM.router.getParam(this.moduleObject.routerId)
        }) || {}
}
IDM.datasource.request(this.propData?.dataSource?.[0]?.id, {
    moduleObject: this.moduleObject,
    param: {
        ...params,
        ...this.chooseTabParams, // 接口tab传来的参数
        limit: this.propData.limit,
        start: this.currentPage
    }
})
```

##### 接口返回数据格式

```js
{
    code: 200,
    data: {
        value: [
            {
                jumpUrl: '',
                title: '黄浦区半淞园路街道：党员先锋我先行，助力进博添风采',
                origin: '黄埔区委组织部',
                time: '2022-07-22',
                images: []
            },
            {
                jumpUrl: '',
                title: '深入学习贯彻党的十九届六中全会精神',
                origin: '上海发布',
                time: '2021-12-22',
                images: [IDM.url.getModuleAssetsWebPath(require('../assets/banner1.jpg'), _this.moduleObject)]
            },
            {
                jumpUrl: '',
                title: '闵行莘庄：“莘长征路上” 再出发  深化“两新”党员学党史, 闵行莘庄：“莘长征路上” 再出发  深化“两新”党员学党史, 闵行莘庄：“莘长征路上” 再出发  深化“两新”党员学党史',
                origin: '闵行区委组织部',
                time: '2022-01-22',
                images: [
                    IDM.url.getModuleAssetsWebPath(require('../assets/banner3.jpg'), _this.moduleObject),
                    IDM.url.getModuleAssetsWebPath(require('../assets/banner2.jpg'), _this.moduleObject),
                    IDM.url.getModuleAssetsWebPath(require('../assets/banner1.jpg'), _this.moduleObject)
                ]
            }
        ],
        count: 3,
        moreUrl: ''
    },
    message: 'success'
}
```
