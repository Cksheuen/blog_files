# 1.用户接口

请求根目录：`http://127.0.0.1:3000/`

## 1.1 登录

- 请求路径: login
- 请求方法: post
- 请求参数：

| 参数名   | 参数说明              | 备注     |
| -------- | --------------------- | -------- |
| username | 用户名（3到15个字符） | 不能为空 |
| password | 密码（6到16个字符）   | 不能为空 |

- 相应参数

| 参数名 | 参数说明 | 备注            |
| ------ | -------- | --------------- |
| token  | 令牌     | 基于 jwt 的令牌 |

- 响应数据

```json
{
    "data": {
        "token": "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1aWQiOjUwMCwicmlkIjowLCJpYXQiOjE1MTI1NDQyOTksImV4cCI6MTUxMjYzMDY5OX0.eGrsrvwHm-tPsO9r_pxHIQ5i5L1kX9RX444uwnRGaIM"
    },
    "meta": {
        "msg": "登录成功",
        "status": 200
    }
}，
{
    "data": null,
    "meta": {
        "msg": "登陆失败",
        "status": 404
    }
}
```

## 1.2 用户管理

### 1.2.1 用户数据列表

- 请求路径: users
- 请求方法: get
- 请求参数

| 参数名   | 参数说明     | 备注                  |
| -------- | ------------ | --------------------- |
| query    | 查询参数     | 为空（未开发相应功能) |
| pagenum  | 当前页码     | 不能为空              |
| pagesize | 每页显示条数 | 不能为空              |

- 相应参数

| 参数名  | 参数说明 | 备注 |
| ------- | -------- | ---- |
| total   | 用户总数 |      |
| pagenum | 当前页码 |      |
| users   | 用户数据 |      |

- 响应数据

```json
{
    "data": {
        "total": 5,
        "pagenum": 4,
        "users": [
            {
                "id": 25,
                "username": "tige117",
                "email": "tige112@163.com",
                "rights": "管理员"
            }
        ]
    },
    "meta": {
        "msg": "获取成功",
        "status": 200
    }
},
{
    "data": null,
    "meta": {
        "msg": "获取失败",
        "status": 404
    }
}
```

### 1.2.2 添加用户

- 请求路径: users
- 请求方法: post
- 请求参数

| 参数名   | 参数说明 | 备注     |
| -------- | -------- | -------- |
| username | 用户名称 | 不能为空 |
| password | 用户密码 | 不能为空 |
| email    | 邮箱     | 可以为空 |

- 响应参数

| 参数名   | 参数说明                 | 备注 |
| -------- | ------------------------ | ---- |
| id       | 用户 ID                  |      |
| username | 用户名                   |      |
| email    | 邮箱                     |      |
| rights   | 管理权限(默认都是管理员) |      |

```json
{
  "data": {
    "id": 566,
    "username": "admin",
    "email": "bb@itcast.com",
	"rights": "管理员"
  },
  "meta": {
    "msg": "添加用户成功",
    "status": 200
  }
}
```

### 1.2.3 删除用户

- 请求路径： users/:id
- 请求方法: delete
- 请求参数

| 参数名 | 参数说明 | 备注                       |
| ------ | -------- | -------------------------- |
| id     | 用户 id  | 不能为空`参数是url参数:id` |

- 响应数据

```json
{
	"data": null,
    "meta": {
        "msg": "删除成功",
        "status": 200
    }
}
```

# 2.文字修改接口

## 2.1 获取文字

- 请求路径： character
- 请求方法: get
- 响应数据

```json
{
    "data": {
        "character": {
            "about": "<p>2009年,依托于国家级实验教学⽰范中⼼电⼦电⼯实验中⼼、国家级电⼦信息技术虚拟仿真实验中⼼,与美国微芯公司合作,共同成⽴了微芯-杭电⼤学⽣科技创新孵化器。</p><p>孵化器实验室团队有三个目标：孵化创新型⼈才,孵化创新型产品,孵化创新型科技公司。三个孵化,三个创新,是我们这个团队凝聚在⼀起的缘由,也是我们⼀直坚持努⼒的动⼒。</p>"，
            "glory": "<p>全校唯一被授予“全国小平科技创新团队”的实验室(全国50个，浙江省2个)。</p><p>发表科技论文8篇,申请专利十余项</p><p>获电子设计、FPGA创新大赛、数学建模、数模美赛等国家级、国际级竞赛奖项四十余人次,获省级以上竞赛奖项百余人次。</p><p>省新苗4项，国家级大创10余项，院芯苗20余项对外合作项目 3 项。</p><p>多次在互联网+、电商、大小挑等比赛中负责技术指导和技术实现。</p>"
        }    
    }，
    "meta": {
    	"msg": "获取文字成功",
    	"status": 200
	}
}
```

## 2.2 修改文字

- 请求路径: character/:type
- 请求方法: put
- 请求参数

| 参数名  | 参数说明                     | 备注                                |
| ------- | ---------------------------- | ----------------------------------- |
| type    | 修改的是简介文字还是荣誉文字 | 不能为空（简介为about,荣誉为glory） |
| content | 修改的内容                   | 不能为空                            |

- 响应数据

```json
{
    "data": null,
    "meta": {
        "msg": "修改成功",
        "stauts": 200
    }
}
```

# 3.图片修改

## 3.1 获取图片

- 请求路径: image
- 请求方法: get
- 响应数据:

```json
{
    "data": {
        "home": {
            "background": "http://xxx",
            "main": []
        },
        "about": {
          	"background": "http://xxx",
         	"main": [
                "http://xxx",
                "http://xxx",
                "http://xxx"
            ]
        },
        "glory": {
        	"background": "http://xxx",
            "main": [
                "http://xxx",
                "http://xxx",
                "http://xxx"
            ]
        },
        "join": {
            "background": "http://xxx",
            "main": {
                "wechat": "http://xxx",
            	"qq": "http://xxx",
            	"oj": "http://xxx"
            }
        }
    },
    "meta": {
        "msg": "获取图片成功",
        "status": 200
    }
}
```

## 3.2 上传图片

- 请求路径: image/:type/:key
- 请求方法： put
- 请求参数

| 参数名 | 参数说明                                         | 备注                                     |
| ------ | ------------------------------------------------ | ---------------------------------------- |
| type   | 图片上传至那个模块(home、about、glory、join)     | 必填                                     |
| key    | 图片类型(background、main)、home 的 main暂时没有 | 必填                                     |
| imgURL | base 64格式的图片类型                            | 必填                                     |
| prop   | 属性                                             | type 为 join 时填写，值有 wechat、qq、oj |

- 响应参数

```json
{
    "data": null,
    "meta": {
        "msg": "上传图片成功",
        "status": 200
    }
}
```

# 4.链接

## 4.1 获取链接

- 请求路径: link
- 请求方法: get
- 响应数据

```json
{
    "data": {
        "site": [
            { id: 1, title: 'OJ 平台', website: "http://xxx" }
        ]
    }
    "meta": {
        "msg": "获取连接成功",
        "status": 200
    }
}
```

## 4.2 修改链接

- 请求路径: link/:type
- 请求方法： put
- 请求参数

| 参数 | 参数说明                       | 备注                      |
| ---- | ------------------------------ | ------------------------- |
| site | 修改的链接                     |                           |
| type | 修改那个网站的链接(目前只有oj) | 现阶段直接 link/oj 就行了 |

- 响应数据

```json
{
	"data": null,
    "meta": {
        "msg": "修改链接成功",
        "status": 200
    }
}
```

