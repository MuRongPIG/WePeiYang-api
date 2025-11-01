# 微北洋客户端api

## 前置说明
| api分类 | 地址 |
| ---- | ---- |
| 主api | `https://api.twt.edu.cn/api/`|
| 论坛api（青年湖底）| `https://qnhd.twt.edu.cn/api/v1/f/` |
| 教室查询（自学） | `https://selfstudy.twt.edu.cn` |
| 升级信息获取| `https://upgrade.twt.edu.cn` |
|~~未知/废弃~~| `https://haitang.twt.edu.cn` |

- 特殊请求头参数（对于主api）：
  ```json
  {
    "domain":"weipeiyang.twt.edu.cn",
    "ticket":"YmFuYW5hLjM3YjU5MDA2M2Q1OTM3MTY0MDVhMmM1YTM4MmIxMTMwYjI4YmY4YTc="
  }
  ```
- 微北洋应用端使用ua：`Dart/3.3 (dart:io)`
- __该api仅可在校园网环境下使用__

## 身份验证

### 使用密码登录

- 请求url： `/auth/common`
- 请求方法：`post`
- 请求体参数说明：

| 参数名 |解释 |
| ----  | --- |
| account | 学号/昵称/邮箱/手机号|
| password | 密码 | 

- 响应示例：

```json
{
    "error_code": 0,
    "message": "成功",
    "result": {
        "userNumber": "3025244999",
        "nickname": "法外狂徒",
        "telephone": "13333333333",
        "email": "no@thankyou.com",
        "token": "******",
        "role": "普通用户",
        "realname": "张三",
        "gender": "男",
        "department": "智能与计算学部",
        "major": "工科试验班(智能与计算类)",
        "stuType": "本科生",
        "avatar": "",
        "campus": "",
        "idNumber": "114514191908100001",
        "area": "未查询到宿舍信息",
        "building": "未查询到宿舍信息",
        "floor": "未查询到宿舍信息",
        "room": "未查询到宿舍信息",
        "bed": "未查询到宿舍信息",
        "upgradeNeed": []
    }
}
```

- 响应参数说明：

| 参数名 |解释 |
| ----  | --- |
| error_code | 错误代码，若成功则为0|
| message | 提示信息，若成功则为“成功” | 
| userNumber | 学号 |
| nickname | 昵称 |
| telephone | 电话号码 |
| email | 邮箱 |
| token | 本次登录的凭据 |
| role | 用户类型 |
| realname | 真实姓名 |
| gender | 性别 |
| department | 学院 |
| major | 专业 |
| stuType | 学生类别 |
| avatar | 头像（~~疑似废弃并~~被论坛api替代），仅返回空字符串|
| campus | 校区（~~疑似废弃~~），仅返回空字符串 |
| idNumber | 身份证号 |
| area| 寝室区域（~~疑似废弃~~），仅返回“未查询到宿舍信息” |
| building|寝室楼（~~疑似废弃~~），仅返回“未查询到宿舍信息” |
| floor|寝室楼层（~~疑似废弃~~），仅返回“未查询到宿舍信息” |
| room |寝室房间号（~~疑似废弃~~），仅返回“未查询到宿舍信息”|
| bed |床位号（~~疑似废弃~~），仅返回“未查询到宿舍信息” |
| upgradeNeed |是否为同校本科生账号需要升级为研究生（待测试补充） |

###### 注意：除 error_code 和 message 外，其他信息均包含在 result 内

### 获取手机验证码（登录/忘记密码/注册）
- 请求url： `/password/reset/msg?phone={phone}`
- 请求方法：`post`
- **URL**参数说明：

| 参数名 |解释 |
| ----  | --- |
| phone | 手机号码 |

- 响应示例：
```json
{
    "error_code": 0,
    "message": "成功",
    "result": null
}
```
- 响应参数说明：

| 参数名 |解释 |
| ----  | --- |
| error_code | 错误代码，若成功则为0|
| message | 提示信息，若成功则为“成功” | 
| result | null |

- #### 该请求会返回一个名为 JESSIONID 的 cookie，后续使用验证码相关请求时需加上该cookie。

### 使用手机验证码登录

- 请求url： `/auth/phone`
- 请求方法：`post`
- 请求参数说明：

| 参数名 |解释 |
| ----  | --- |
| phone | 手机号码 |
| code | 验证码 |

- 响应示例：同“使用密码登录”
- 响应参数说明：同“使用密码登录”

### 忘记密码-验证码验证
- 请求url： `/password/reset/verify`
- 请求方法：`post`
- 请求体参数说明：

| 参数名 |解释 |
| ----  | --- |
| phone | 手机号码 |
| code | 验证码 |

- 响应示例：同“获取手机验证码”
- 响应参数说明：同“获取手机验证码”

### 忘记密码-密码重置（在已完成验证前提下，使用手机号+新密码重置密码）

- 请求url： `/password/reset`
- 请求方法：`post`
- 请求体参数说明：

| 参数名 |解释 |
| ----  | --- |
| phone | 手机号码 |
| password | 新密码 |

- 响应示例：同“获取手机验证码”
- 响应参数说明：同“获取手机验证码”

### 更新token

- 请求url： `/auth/updateToken`
- 请求方法：`post`
- 请求体参数说明：

| 参数名 |解释 |
| ----  | --- |
| token | 旧token |

- 响应示例：
```json
{
    "error_code": 0,
    "message": "成功",
    "result": "***"
}
```
- 响应参数说明：

| 参数名 |解释 |
| ----  | --- |
| error_code | 错误代码，若成功则为0|
| message | 提示信息，若成功则为“成功” | 
| result | 新token |

