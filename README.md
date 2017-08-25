|版本/类型|作者|参与者|日期|描述|
|:---:|:---:|:---:|:---:|:---:|
|1.0/草稿|Pr0ph3t||2017.7.27|初始化接口|
|1.01/草稿|Pr0ph3t||2017.8.25|更正前台接口|

# Quanta官网V1.0接口

#### 目录:
- [一、备注](#一备注)
 - [1. 接口请求规范](#1-接口请求规范)
 - [2. 请求参数说明](#2-请求参数说明)
 - [3. 返回参数说明](#3-返回参数说明)
- [二、前台部分](#二前台部分)
 - [1. 根据届份获取管理人员](#1-根据届份获取管理人员)
 - [2. 发送邮件](#2-发送邮件)
 - [3. 获取动态年份](#3-获取动态年份)
 - [4. 根据年份获取动态](#4-根据年份获取动态)
 - [5. 根据平台获取项目](#5-根据平台获取项目)
 - [6. 根据项目id获取项目详情](#6-根据项目id获取项目详情)
- [三、后台部分](#三后台部分)
 - [待定](#待定)

-------

## 一、备注 ##

##### 1. 接口请求规范 #####

|url|支持请求格式|HTTP请求方式|是否要求登陆验证|请求频率限制|
|:---:|:---:|:---:|:---:|:---:|
|X|JSON/None|GET/POST|False|(0表示无限制)15次/s|
Ps：参数有可能会出现在Url内，如``/GetMemberList/12``即表示请求12届名单;
每个POST请求都要带上CSRF Token以预防CSRF攻击
Token在每个Form表单内都会带有一个input标签
类似于：
```HTML
<input type="hidden" name="_token" value="oi8sRqjFFk6pYGLTkHEACd4wjd9TX6IFOn3jqQBI">
```

##### 2. 请求参数说明 #####

|参数名|必选|类型及范围|说明|
|:---:|:---:|:---:|:---:|
|XXX|True|String(13)|用户名|
|||||
|||||


##### 3. 返回参数说明 #####
|返回键|类型|返回值|说明|
|:---:|:---:|:---:|:---:|:---:|
|code|string|2000|返回结果代号|
|Response|array||响应体|

Code返回结果代号：
|代号|说明|
|:---:|:---:|
|2000|成功|
|1000|服务器处理失败|
|1001|HTTP请求方式错误|
|1002|请求参数格式错误|
|1003|未授权访问|

其中Response中包含响应数据，具体格式请查看具体接口。


自定义Http状态码：
|代号|说明|
|:---:|:---:|
|429|访问太频繁|
|...|...|


自定义Http头：
|键|说明|
|:---:|:---:|
|X-RateLimit-Limit|每分钟内允许请求的最大次数|
|X-RateLimit-Remaining|这分钟内允许请求的剩余次数|
|Retry-After|距离下次请求剩余时间(s)|
|...|...|


------


## 二、前台部分 ##
##### 1. 根据届份获取管理人员 #####

- 请求格式
 |url|支持请求格式|HTTP请求方式|是否要求登陆验证|请求数限制|
 |:---:|:---:|:---:|:---:|:---:|
 |/member/get/{届数}|None|GET|False|0|

- 请求参数
 |参数名|必选|类型及范围|说明|
 |:---:|:---:|:---:|:---:|
 |届数|True|int(4)|在Url内填写，参考请求格式|

- 返回参数说明
 |返回键|类型|返回值|说明|
 |:---:|:---:|:---:|:---:|:---:|
 |code|string|2000|返回结果代号|
 |Response|array||响应体，为空|

 - Example：
```JSON
{
    'code' : '2000',
    'Response' : [
                'gen' : 12, //届份
                'core' : [
                        'CEO' : [
                            'name' : 'zzj',
                            'grade' : '2015级',
                            'major' : '网络工程',
                            'pic' : 'img',
                        ],
                        'CTO' : [
                            'name' : 'lgb',
                            'grade' : '2015级',
                            'major' : '软件工程',
                            'pic' : 'img',
                        ],
                    ], //核心管理层
                'operation' : [], //运营部
                'RnD' : [], //研发部
                'design' : [] //设计部
            ]
}
```

##### 2. 发送邮件 #####

- 请求格式
 |url|支持请求格式|HTTP请求方式|是否要求登陆验证|请求频率限制|
 |:---:|:---:|:---:|:---:|:---:|
 |/mail/post|JSON|POST|False|30次/Min|

- 请求参数
 |参数名|必选|类型及范围|说明|
 |:---:|:---:|:---:|:---:|
 |fromName|False|String(64)|发送者署名，可为空|
 |feedbackEmail|True|String(64)|Email格式|
 |title|False|String(32)|主题，可为空|
 |content|True|String(128)|发送内容|

- 返回参数说明
 |返回键|类型|返回值|说明|
 |:---:|:---:|:---:|:---:|:---:|
 |code|string|2000|返回结果代号|
 |Response|array||响应体，为空|

 - Example：
```JSON
{
    'code' : '2000',
    'Response' : [] //为空
}
```

##### 3. 获取动态年份 #####

- 请求格式
 |url|支持请求格式|HTTP请求方式|是否要求登陆验证|请求频率限制|
 |:---:|:---:|:---:|:---:|:---:|
 |/news/getYears|None|GET|False|0|

- 请求参数
无

- 返回参数说明
 |返回键|类型|返回值|说明|
 |:---:|:---:|:---:|:---:|:---:|
 |code|string|2000|返回结果代号|
 |Response|array||响应体,格式如下|

 - Example：
```JSON
{
    'code' : '2000',
    'Response' : [
            '2017',
            '2016',
            '2015',
        ]
}
```

##### 4. 根据年份获取动态 #####

- 请求格式
 |url|支持请求格式|HTTP请求方式|是否要求登陆验证|请求频率限制|
 |:---:|:---:|:---:|:---:|:---:|
 |/news/getByYear/{年份}|None|GET|False|0|

- 请求参数
 |参数名|必选|类型及范围|说明|
 |:---:|:---:|:---:|:---:|
 |年份|True|int(4)|在Url内填写，参考请求格式|

- 返回参数说明
 |返回键|类型|返回值|说明|
 |:---:|:---:|:---:|:---:|:---:|
 |code|string|2000|返回结果代号|
 |Response|array||响应体,格式如下|

 - Example：
```JSON
{
    'code' : '2000',
    'Response' : [
            [
                'title' : 'Quanta第十二届管理层换届公告',
                'pic' : 'img',
                'author' : 'OldFe'
            ],
            [
                'title' : 'Quanta第十二届管理层换届公告',
                'pic' : 'img',
                'author' : 'OldFe'
            ],
        ]
}
```

##### 5. 根据平台获取项目 #####

- 请求格式
 |url|支持请求格式|HTTP请求方式|是否要求登陆验证|请求频率限制|
 |:---:|:---:|:---:|:---:|:---:|
 |/project/get/{平台}|None|GET|False|0|

- 请求参数
 |参数名|必选|类型及范围|说明|
 |:---:|:---:|:---:|:---:|
 |平台名称|False|Varchar(10)|默认为获取全部, 可选值: Web,Andorid,IOS,MiniProgram,Other|

- 返回参数说明
 |返回键|类型|返回值|说明|
 |:---:|:---:|:---:|:---:|:---:|
 |code|string|2000|返回结果代号|
 |Response|array||响应体,格式如下|


 - Example：
```JSON
{
    'code' : '2000',
    'Response' : [
            [
                'id' : 1,
                'name' : '艺术+',
                'type' : 'Web|Andorid'
                'pic' : 'img',
                'time' : '2015-12-23'
                'productManager' : '蟑螂'
            ],
            [
                'id' : 2,
                'name' : '映像派',
                'type' : 'Andorid'
                'pic' : 'img',
                'time' : '2015-12-23'
                'productManager' : '蟑螂'
            ],
            [
                'id' : 3,
                'name' : 'XXX',
                'type' : 'Web|IOS'
                'pic' : 'img',
                'time' : '2015-12-23'
                'productManager' : '蟑螂'
            ],
            [
                'id' : 4,
                'name' : 'YiCheng',
                'type' : 'MiniProgram'
                'pic' : 'img',
                'time' : '2015-12-23'
                'productManager' : '蟑螂'
            ],
            [
                'id' : 5,
                'name' : 'AAA',
                'type' : 'Other'
                'pic' : 'img',
                'time' : '2015-12-23'
                'productManager' : '蟑螂'
            ],
        ]
}
```

//一个项目可以同时拥有多个type，多个type使用 | 分割

##### 6. 根据项目id获取项目详情 #####

- 请求格式
 |url|支持请求格式|HTTP请求方式|是否要求登陆验证|请求频率限制|
 |:---:|:---:|:---:|:---:|:---:|
 |/project/getById/{id}|None|GET|False|0|

- 请求参数
 |参数名|必选|类型及范围|说明|
 |:---:|:---:|:---:|:---:|
 |id|True|int(4)|请求的项目id|

- 返回参数说明
 |返回键|类型|返回值|说明|
 |:---:|:---:|:---:|:---:|:---:|
 |code|string|2000|返回结果代号|
 |Response|array||响应体,格式如下|

 - Example：
```JSON
{
    'code' : '2000',
    'Response' : [
                'name' : '艺术+',
                'description' : 'XXXXXXX',
                'tools' : 'HTML|JS|JQery|PHP',
                'time' : '2015-05-23'
                'Web' : [
                        'pic' : ['img1','img2'],
                        'group' : [
                            'productManager' : 'XXX',
                            'operationManager' : 'XXX',
                            'frontend' : 'XXX|YYY|JJJ',
                            'backend' : 'AAA|BBB|CCC',
                            'designer' : 'QQQ|PPP',
                         ],
                        'website' : 'www.fuck.com' 
                    ],
                'Android' : [
                        'pic' : ['img1','img2'],
                        'group' : [
                            'productManager' : 'XXX',
                            'operationManager' : 'XXX',
                            'frontend' : 'XXX|YYY|JJJ',
                            'backend' : 'AAA|BBB|CCC',
                            'designer' : 'QQQ|PPP',
                         ],
                        'website' : 'www.fuck.com' 
                    ],
                'IOS' : [
                        'pic' : ['img1','img2'],
                        'group' : [
                            'productManager' : 'XXX',
                            'operationManager' : 'XXX',
                            'frontend' : 'XXX|YYY|JJJ',
                            'backend' : 'AAA|BBB|CCC',
                            'designer' : 'QQQ|PPP',
                         ],
                        'website' : 'www.fuck.com' 
                    ],
        ]
}
```
//多名人员使用 | 分割
