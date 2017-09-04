|版本/类型|作者|参与者|日期|描述|
|:---:|:---:|:---:|:---:|:---:|
|1.0/草稿|Pr0ph3t||2017.7.27|初始化接口|
|1.01/草稿|Pr0ph3t||2017.8.25|更正前台接口|
|2.0|ben|Pr0ph3t|2017.8.31|重新拟定前台接口|
|2.1|ben||2017.9.4|修复获取管理层简介接口参数错误问题|

# Quanta官网V2.1接口

#### 目录:
- [一、备注](#一备注)
  - [1. 接口请求规范](#1-接口请求规范)
  - [2. 请求参数说明](#2-请求参数说明)
  - [3. 返回参数说明](#3-返回参数说明)
- [二、前台部分](#二前台部分)
  - [1. 获取动态年份](#1-获取动态年份)
  - [2. 获取动态列表](#2-获取动态列表)
  - [3. 获取动态详情](#3-获取动态详情)
  - [4. 获取项目列表](#4-获取项目列表)
  - [5. 获取项目详情](#5-获取项目详情)
  - [6. 获取当前届数](#6-获取当前届数)
  - [7. 获取管理层简介](#7-获取管理层简介)
  - [8. 发送邮件](#8-发送邮件)
- [三、后台部分](#三后台部分)
  - [待定](#待定)

-------

## 一、备注 ##

##### 1. 接口请求说明 #####

参数有可能会出现在Url内，如``/GetMemberList/12``即表示请求12届名单;
每个POST请求都要带上CSRF Token以预防CSRF攻击
Token在每个Form表单内都会带有一个input标签
类似于：
```
HTML:
<input type="hidden" name="_token" value="oi8sRqjFFk6pYGLTkHEACd4wjd9TX6IFOn3jqQBI">
```

##### 2. 请求参数说明 #####

|参数名|类型|是否必选|说明|
|:---:|:---:|:---:|:---:|
|XXX|string|是|xxxx|

##### 3. 响应参数说明 #####

|返回键|类型|返回值|说明|
|:---:|:---:|:---:|:---:|
|code|string|2000|返回结果代号|
|response|object||响应体|

- Code返回结果代号：

|代号|说明|
|:---:|:---:|
|2000|成功|
|1000|服务器处理失败|
|1001|HTTP请求方式错误|
|1002|请求参数格式错误|
|1003|未授权访问|

其中response中包含响应数据，具体格式请查看具体接口。

- 自定义Http状态码：

|代号|说明|
|:---:|:---:|
|429|访问太频繁|

- 自定义Http头：

|键|说明|
|:---:|:---:|
|X-RateLimit-Limit|每分钟内允许请求的最大次数|
|X-RateLimit-Remaining|这分钟内允许请求的剩余次数|
|Retry-After|距离下次请求剩余时间(s)|


## 二、前台部分 ##
##### 1. 获取动态年份 #####
- 描述：获取存在动态的年份
- 请求地址：`/getYears`
- 请求方式：get
- 请求参数：无
- 响应参数：

|参数名|类型|说明|
|:-:|:-:|:-:|
|years|array(string)|存在有动态的年份的列表（倒序）|

- 响应示例：
```
{
    "code": "2000",
    "response": {
        "years": [
            "2017",
            "2016",
            "2015",
            "2014",
            "2013",
            "2012",
            "2011",
            "2010",
            "2009"
        ]
    }
}
```

##### 2. 获取动态列表 #####
- 描述：根据年份获取动态列表
- 请求地址：`/getTrends`
- 请求方式：get
- 请求参数：

|参数名|类型|是否必选|说明|
|:-:|:-:|:-:|:-:|
|year|string|是|动态的年份|

- 请求示例：
```
{
    "year": "2017"
}
```

- 响应参数：

|参数名|类型|说明|
|:-:|:-:|:-:|
|trends|array(object)|特定年份的动态列表|
|id|string|动态ID|
|title|string|标题|
|author|string|作者|
|date|string|日期|
|cover|object|封面信息|
|src|string|图片地址|

- 响应示例：
```
{
    "code": "2000",
    "response": {
        "trends": [{
            "id": "1",
            "title": "Quanta第十二届管理层换届公告",
            "author": "OldFe",
            "date": "2017/06/24",
            "cover": "../img/picture.jpg",
        }]
    }
}
```

##### 3. 获取动态详情 #####
- 描述：根据动态ID获取动态详情
- 请求地址：`/getTrendInfo`
- 请求方式：get
- 请求参数：

|参数名|类型|是否必选|说明|
|:-:|:-:|:-:|:-:|
|id|string|是|动态ID|

- 请求示例：
```
{
    "id": "1"
}
```

- 响应参数：

|参数名|类型|说明|
|:-:|:-:|:-:|
|trends|array(object)|特定年份的动态列表|
|title|string|标题|
|author|string|作者|
|date|string|日期|
|cover|object|封面信息|
|src|string|图片地址|
|content|string|内容|

- 响应示例：
```
{
    "code": "2000",
    "response": {
        "id": "1",
        "title": "Quanta第十二届管理层换届公告",
        "author": "OldFe",
        "date": "2017/06/24",
        "cover": "../img/picture.jpg",
        "content": "htmlstring"
    }
}
```

##### 4. 获取项目列表 #####
- 描述：根据类型获取项目列表
- 请求地址：`/getProjects`
- 请求方式：get
- 请求参数：

|参数名|类型|是否必选|说明|
|:-:|:-:|:-:|:-:|
|type|string|是|项目类型（包括全部（all）、网站（web）、安卓（android）、苹果（ios）、小程序（mini apps）、其他平台（else））|

- 请求示例：
```
{
    "type": "mini apps"
}
```

- 响应参数：

|参数名|类型|说明|
|:-:|:-:|:-:|
|id|string|项目ID|
|projects|array(object)|特定类型的项目列表|
|name|string|项目名|
|pm|string|产品经理|
|date|string|最新项目日期|
|cover|object|封面地址|
|platforms|array(string)|平台类型（包括网站（web）、安卓（android）、苹果（ios）、小程序（mini apps）、其他平台（else））|

- 响应示例：
```
{
    "code": "2000",
    "response": {
        "projects": [{
            "id": "1",
            "name": "艺术+",
            "pm": "OldFe",
            "date": "2017/06/24",
            "cover": "../img/picture.jpg",
            "platforms": [
                "web",
                "android"
            ]
        }]
    }
}
```

##### 5. 获取项目详情 #####
- 描述：根据项目ID获取项目详情
- 请求地址：`/getProjectInfo`
- 请求方式：get
- 请求参数：

|参数名|类型|是否必选|说明|
|:-:|:-:|:-:|:-:|
|id|string|是|项目ID|

- 请求示例：
```
{
    "id": "1"
}
```

- 响应参数：

|参数名|类型|说明|
|:-:|:-:|:-:|
|id|string|项目ID|
|name|string|项目名|
|pm|string|产品经理|
|date|string|最新项目日期|
|cover|string|封面地址|
|platforms|array(string)|平台类型（包括网站（web）、安卓（android）、苹果（ios）、小程序（mini apps）、其他平台（else））|
|info|object|项目详情|
|type|string|项目类型（平台）|
|profile|string|项目简介|
|tools|string|工具|
|date|string|日期|
|website|string|地址|
|developers|array(object)|开发人员|
|post|string|职位|

- 响应示例：
```
{
    "code": "2000",
    "response": {
        "id": "1",
        "name": "艺术+",
        "pm": "张**",
        "date": "2015/05/13",
        "cover": "../img/picture.jpg",
        "platforms: [
            "web",
            "mini app",
            "android"
        ],
        "info": [{
            "type": "web",
            "name": "艺术+",
            "profile": "Quanta（量子）信息技术服务中心是以半企业化模式运营的一个专业的IT技术组织，致力于大型的商业或非商业项目开发、技术攻关与自主产品研发工作。以育人文化为宗旨，Quanta秉持着“Nothing but professional.”的原则，培育出一届又一届优秀的IT人才。",
            "tools": "js php html css jquery",
            "date": "2015/05/13",
            "developers": [{
                "name": "张**",
                "post": "产品经理"
            }, {
                "name": "曾**",
                "post": "后台工程师"
            }],
            "website": "https://www.baidu.com"
        }, {
            "type": "android",
            "name": "艺术+",
            "profile": "Quanta（量子）信息技术服务中心是以半企业化模式运营的一个专业的IT技术组织，致力于大型的商业或非商业项目开发、技术攻关与自主产品研发工作。以育人文化为宗旨，Quanta秉持着“Nothing but professional.”的原则，培育出一届又一届优秀的IT人才。",
            "tools": "php java",
            "date": "2015/05/13",
            "developers": [{
                "name": "张**",
                "post": "产品经理"
            }, {
                "name": "曾**",
                "post": "后台工程师"
            }],
            "website": ""
        }]
    }
}
```

##### 6. 获取当前届数 #####
- 描述：获取当前年份第几届
- 请求地址：`/getSessions`
- 请求方式：get
- 请求参数：无

- 响应参数：

|参数名|类型|说明|
|:-:|:-:|:-:|
|sessions|array(string)|所有届份|

- 响应示例：
```
{
    "code": "2000",
    "response": {
        "sessions": [
            "12",
            "11",
            "10",
            "9",
            "8"
        ]
    }
}
```

##### 7. 获取管理层简介 #####
- 描述：根据届份获取管理层简介
- 请求地址：`/getAdministrators`
- 请求方式：get
- 请求参数：

|参数名|类型|是否必选|说明|
|:-:|:-:|:-:|:-:|
|session|string|是|届数|

- 请求示例：
```
{
    "session": "12"
}
```

- 响应参数：

|参数名|类型|说明|
|:-:|:-:|:-:|
|administrators|array(object)|管理层介绍|
|type|string|类型（包括核心管理层、运营部、设计部与研发部）|
|name|string|姓名|
|post|string|职位|
|avatar|string|头像地址|
|session|object|届份|
|current|boolean|是否为当届成员|
|content|string|内容|
|series|string|年级|
|major|string|专业|

- 响应示例：
```
{
    "code": "2000",
    "response": {
        "administrators": [{
            "type": "核心管理层",
            "staff": [{
                "name": "曾**",
                "post": "Quanta第12届CEO",
                "avatar": "../img/picture.jpg",
                "session": {
                    "current": true,
                    "content": "12"
                },
                "series": "2015",
                "major": "软件工程",
            }]
        }, {
            "type": "运营部",
            "staff": [{
                "name": "曾**",
                "post": "Quanta第12届运营部经理",
                "avatar": "../img/picture.jpg",
                "session": {
                    "current": true,
                    "content": "12"
                },
                "series": "2015",
                "major": "软件工程",
            }]
        }, {
            "type": "设计部",
            "staff": [{
                "name": "曾**",
                "post": "Quanta第12届设计部经理",
                "avatar": "../img/picture.jpg",
                "session": {
                    "current": true,
                    "content": "12"
                },
                "series" : "2015",
                "major": "软件工程",
            }]
        }, {
            "type": "研发部",
            "staff": [{
                "name": "曾**",
                "post": "Quanta第12届研发部经理",
                "avatar": "../img/picture.jpg",
                "session": {
                    "current": true,
                    "content": "12"
                },
                "series": "2015",
                "major":  "软件工程",
            }]
        }]
    }
}
```

##### 8. 发送邮件 #####
- 描述：发送联系邮件
- 请求地址：`/sendEmail`
- 请求方式：post
- 请求参数：

|参数名|类型|是否必选|说明|
|:-:|:-:|:-:|:-:|
|name|string|是|姓名|
|email|string|是|邮箱|
|phone|string|否|手机|
|title|string|是|邮件主题|
|content|string|是|邮件内容|

- 请求示例：
```
{
    "name": "曾**",
    "email": "123456789@qq.com,
    "phone": "12345678912",
    "title": "邮件主题",
    "content": "邮件内容"
}
```

- 响应参数：无

- 响应示例：
```
{
    "code": "2000",
    "response": {}
}