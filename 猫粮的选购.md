# 欢迎使用定制猫粮

------
> * 猫咪的发展和进化已有340万年，现有品种42个（美国CFA标准）；
> *基因层面上很大程度决定了猫咪的潜在健康缺陷；
> *根据身体不同形态、运动量的大小、季节的变化、特殊体质的差异，定制猫粮是有必要的！

### 我们已知的重要要指标：
| 蛋白质 | 40%以上更好，33~35%是底线 | ​关键 |
| --- | --- | --- |
| 脂肪 | 猫粮少见过高，一般20%左右 | / |
| 磷含量 | 越高意味着骨含量大，肉粉多
>1.4%=肉粉、骨骼必有其一，>1.8%算高 | 重要​ |
| 纤维 | 越多则消化率下降粪便变多 | 重要​ |
| 牛磺酸 | 少见不合格，无所谓 | / |
| 灰分 | 无机物，越多意味着肉粉多 | 重要​ |
| 蛋白质高不软便嘛？ | 高蛋白是必须的，40%？不算高，还可以再高。 | 重要​ |
| 肉粉=垃圾？ | 肉粉有好有坏，看供应商； | 重要​ |
| 植物蛋白=猫不消化？ | 没提纯、没水解不能完全消化的蛋白质，但是一般难区分！ |  |



这里提供了一款科学养猫为核心理念，使用微信小程序实现定制猫粮的产品：

<img src="https://cdn.nlark.com/yuque/0/2021/png/718838/1628836034207-a2d86d67-eb2e-4780-9a9b-fb8af27db9e8.png" alt="图片" width="400" height="400" align=center />

专属定制的猫粮，想了解更多可以看#[这篇文章](https://mp.weixin.qq.com/s/kn-4l9DGyaNNnf3IiBnyoQ)！
------

## 什么是定制猫粮
> * 如果你会简单的GET、POST请求，
> 那么你可以自己尝试一下生成一个专属的猫咪配方！
## 1.用户发起订单 
- app端:用户发起定制，post请求接口，带token及猫咪数据，时序图如下。

![下单时序图.png](https://cdn.nlark.com/yuque/0/2021/png/718838/1610010421283-3659ee5d-5cfe-43a2-add5-d508fa9e9433.png#align=left&display=inline&height=369&margin=%5Bobject%20Object%5D&name=%E4%B8%8B%E5%8D%95%E6%97%B6%E5%BA%8F%E5%9B%BE.png&originHeight=492&originWidth=489&size=22409&status=done&style=none&width=367)


## 请求地址（暂用）


```html
https://peekaboo.top/youmaoapp/addOrder
```


## 入参
### Post参数：
Object 类型，属性如下：

| 名称 | 类型 | 必填 | 说明 | 版本 |
| :--- | :--- | :--- | :--- | :--- |
| **app_key** | String | 是 | 开发者唯一凭证，默认：youmaopeikepet | v1.0 |
| **user_iphone** | Sting | 是 | 用户唯一凭证 | v1.0 |
| **cat_data** | Sting | 是 | 标准json类型，里面是业务参数按照参数 | v1.0 |
| CatName | Sting | 是 | 猫的名字 | 

v1.0 |
| Sex | String | 是 | 性别：仅有"小鲜肉"，"小公主"两种值 |  |
| Birthday | String | 是 | 日期字符串形式，"2018-11-01" |  |
| CatBreed | String | 是 | 猫的品种 |  |
| Weight | String | 是 | 猫体记重单位为**千克** |  |
| ExerciseVolume | String | 是 | 运动状态，慵懒、精神、亢奋 |  |
| BodyShape | String | 是 | 身体形态：瘦弱、适中、肥胖 |  |
| Sterilization | String | 是 | 绝育情况：已绝育、未绝育 |  |
| SpecialStage | String | 是 | 特殊时期：妊娠期、产后期、术后恢复期 |  |
| Healthy | Array | 是 | 健康状态：数组形式， ["被毛光泽", "喝水少"] |  |
| MedicalHistory | Array | 是 | 疾病史：数组形式，["泌尿道疾病", "骨折"] |  |
| **timestamp** | String | 是 | 时间戳， 1561700872可转"%Y-%m-%d %H:%M:%S"
 | v1.0 |



### 请求示例：
```go
{
  "app_key": "youmaopeikepet",
  "user_iphone":"18842615537" ,
  "cat_data": {
                "CatName": "胖欧",
                "Sex": "小公主",
                "Birthday": "2018-11-01",
                "CatBreed": "埃及猫",
                "Weight": "6",
                "ExerciseVolume": "亢奋", 
                "BodyShape": "瘦弱", 
                "Sterilization": "已绝育",
                "SpecialStage":"妊娠期",
                "Healthy": ["被毛光泽", "喝水少"],
                "MedicalHistory": ["泌尿道疾病", "骨折"]
    },
  "timestamp": "1561700872"
}
```


## object.success 回调函数
### data参数
Object 类型，属性如下

| 名称 | 类型 | 描述 |
| :--- | :--- | :--- |
| **order_id** | String | "202001171800125" 订单仅保留10分钟有效期，超时请重新获取。 |
| cat_card | String | 消耗计算卡：{
      "CatName":"胖欧",
      "Weight":"16",
      "age":"8",
      "max_rer":"1080.0",#最大能量
      "min_rer":"864",#最小能量
      "max_protein":"29",#最大蛋白
      "min_protein":"19",#最小蛋白
      "max_fat":"15",#大脂肪
      "min_fat":"15",#小脂肪
      "max_carbohydrate":"15",#大碳水化合物
      "min_carbohydrate":"14",#小碳水化合物
"PList":[
    {"name": "公猫", "text": "车前子0.05%", "checked": ""},
    {"name": "被毛无光泽", "text": "三文鱼油0.5%", "checked": ""},
]
     } |
| formula | String |  #配方配比,配方字段可能变化{
            "chicken": 45.7,
            "salmon": 12.5,  # 三文鱼
            "Shrimp": 6.7,  # 基围虾
            "deer_liver": 3.0,  # 鹿肝
            "duck_meat": 4.3,  # 鸭肉
            "pumpkin": 3.0,  # 南瓜
            "beef": 2.0,  # 牛肉
            "cod": 4.0,  # 鳕鱼
            "fish": 3.8,  # 淡水鱼
             "tunas":20.5,#金枪鱼 

     }，####三文鱼与金枪鱼互相替换标题随key变化，仅可存在一个 |
| order_data | String | 订单数据，为json格式，内含try_eat，solid_food，wet_food，三个模式 |
| try_eat | String | {
                "title":"试吃装",
                  "price":"9.9",#价格
                  "data":"7包干粮",#内容
                  "food_weight":"100",#净重
                  "postage":"",#邮费暂无
} |
| solid_food | String | {
                    "title":"干粮",
                     "cat_name":"加菲",
                      "food_weight":"100",
                      "y_price":"270",
                     "o_data":"[100g*60包]",#内容
                    "o_discount":"9",
                     "o_price":"240",
                  } |
| wet_food | String | {
                    "title":"干粮",
                     "cat_name":"加菲",
                      "food_weight":"100",
                      "y_price":"270",
                     "o_data":"[100g*60包]",#内容
                    "o_discount":"9",
                     "o_price":"240",
                  } |
| err_no | number | 0:接口调用成功; |
| message | String | 返回信息 |



## 示例


```javascript
{
  "order_id": "202001171800125",
  "cat_card": 
    {
       "CatName":"胖欧",
       "Weight":"16",
       "age":"8",
       "max_rer":"1080.0",#最大能量
       "min_rer":"864",#最小能量
       "max_protein":"29",#最大蛋白
       "min_protein":"19",#最小蛋白
       "max_fat":"",#大脂肪
       "min_fat":"",#小脂肪
       "max_carbohydrate":"",#大碳水化合物
       "min_carbohydrate":"",#小碳水化合物
    },
  "formulacat": 
    {
       "chicken": 45.7,
       "salmon": 12.5,  # 三文鱼
       "Shrimp": 6.7,  # 基围虾
       "deer_liver": 3.0,  # 鹿肝
       "duck_meat": 4.3,  # 鸭肉
       "pumpkin": 3.0,  # 南瓜
       "beef": 2.0,  # 牛肉
       "cod": 4.0,  # 鳕鱼
       "fish": 3.8,  # 淡水鱼
       "tunas":20.5,#金枪鱼
    }, 
  "order_data": 
    {
      "try_eat": 
      {
         "title":"试吃装",
         "price":"9.9",#价格
         "data":"7包干粮",#内容
         "food_weight":"100",#净重
         "postage":"",#邮费暂无
      },
    "solid_food": 
      {
        "salmon": 12.5,  # 三文鱼
        "Shrimp": 6.7,  # 基围虾
        "deer_liver": 3.0,  # 鹿肝
        "duck_meat": 4.3,  # 鸭肉
        "pumpkin": 3.0,  # 南瓜
        "beef": 2.0,  # 牛肉
        "cod": 4.0,  # 鳕鱼
        "fish": 3.8,  # 淡水鱼
        "tunas":20.5,#金枪鱼
    },
     "wet_food": 
      {
         "title":"湿粮",
         "cat_name":"加菲",
         "food_weight":"100",#每包净重
         "y_price":"270",
         "o_data":"[100g*60包]",#内容
         "o_discount":"9",
         "o_price":"240",		
    } , 
  "err_no": 0,
  "message": "success"
}
}
```


```html
错误状态：
{
  "err_no": 1,
  "message": "失败，包含错误的请求信息"
}
```
## 
