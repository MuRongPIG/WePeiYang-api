# 微北洋客户端api

## 前置说明

- 微北洋应用端使用ua：`Dart/3.3 (dart:io)`
- __全部api仅可在校园网环境下使用__
- 全部api应使用https访问

| api分类 | 地址 |
| ---- | ---- |
| 主api | `https://api.twt.edu.cn/api/`|
| 论坛api（青年湖底）| `https://qnhd.twt.edu.cn/api/v1/f/` |
| 教室查询（自学） | `https://selfstudy.twt.edu.cn` |
| 升级信息获取| `https://upgrade.twt.edu.cn` |
|~~未知/废弃~~| `https://haitang.twt.edu.cn` |

### - 特殊请求头参数（对于主api）：
  ```json
  {
    "domain":"weipeiyang.twt.edu.cn",
    "ticket":"YmFuYW5hLjM3YjU5MDA2M2Q1OTM3MTY0MDVhMmM1YTM4MmIxMTMwYjI4YmY4YTc="
  }
  ```


### - 为避免重复，此处给出空响应示例：
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


## 身份验证（使用主api）

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
| nickname | 昵称（对应本节中用户名，非论坛区域昵称） |
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

### 获取手机验证码（登录/忘记密码）
- 请求url： `/password/reset/msg?phone={phone}`
- 请求方法：`post`
- **URL**参数说明：

| 参数名 |解释 |
| ----  | --- |
| phone | 手机号码 |

- 响应示例：空响应

- #### 该请求会返回一个名为 JSESSIONID 的 cookie，后续使用验证码相关请求时需加上该cookie。

### 获取手机验证码（登录状态下更新手机号）
- 请求url： `/user/phone/msg`
- 请求方法：`post`
- 请求体参数说明：

| 参数名 |解释 |
| ----  | --- |
| phone | 手机号码 |

- 响应示例：空响应

### 获取手机验证码（注册/补全信息）
- 请求url： `/register/phone/msg?phone={phone}`
- 请求方法：`post`
- **URL**参数说明：

| 参数名 |解释 |
| ----  | --- |
| phone | 手机号码 |

- 响应示例：空响应

### 使用手机验证码登录

- 请求url： `/auth/phone`
- 请求方法：`post`
- 请求体参数说明：

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

- 响应示例：空响应

### 忘记密码-密码重置（在已完成验证前提下，使用手机号+新密码重置密码）

- 请求url： `/password/reset`
- 请求方法：`post`
- 请求体参数说明：

| 参数名 |解释 |
| ----  | --- |
| phone | 手机号码 |
| password | 新密码 |

- 响应示例：空响应

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

### 登录状态下修改密码

- 请求url： `/password/person/reset?password={password}`
- 请求方法：`put`
- **URL**参数说明：

| 参数名 |解释 |
| ----  | --- |
| password | 新密码 |

- 请求体参数说明：

| 参数名 |解释 |
| ----  | --- |
| token | 登录凭据 |

- 响应示例：空响应

###### 微北洋将对旧密码的一致性校验功能做在本地应用实现，因此此处无需旧密码；*这是一种极其不安全的行为!*

### 登录状态下补全信息（第一次登录时）*（待测试/补充）*
- 请求url： `/user/single?telephone={telephone}&verifyCode={verifyCode}&email={email}`
- 请求方法：`put`
- **URL**参数说明：

| 参数名 |解释 |
| ----  | --- |
| telephone | 电话号码 |
| verifyCode | 验证码 |
| email | 邮箱 |

- 请求体参数说明：

| 参数名 |解释 |
| ----  | --- |
| token | 登录凭据 |

- 响应示例：空响应 *（待测试/补充）*

### 登录状态下更新手机号
- 请求url： `/user/single/phone?={phone}&code={code}`
- 请求方法：`put`
- **URL**参数说明：

| 参数名 |解释 |
| ----  | --- |
| phone | 电话号码 |
| code | 验证码 |

- 请求体参数说明：

| 参数名 |解释 |
| ----  | --- |
| token | 登录凭据 |

- 响应示例：空响应

### 登录状态下更新邮箱
- 请求url： `/user/single/email?email={email}`
- 请求方法：`put`
- **URL**参数说明：

| 参数名 |解释 |
| ----  | --- |
| email | 新邮箱 |

- 请求体参数说明：

| 参数名 |解释 |
| ----  | --- |
| token | 登录凭据 |

- 响应示例：空响应

###### 微北洋并未对邮箱真实性进行验证；*这是一种极其不安全的行为!*

### 登录状态下更新用户名（与论坛用户名不同）
- 请求url： `/user/single/username?username={username}`
- 请求方法：`put`
- **URL**参数说明：

| 参数名 |解释 |
| ----  | --- |
| username | 新用户名 |

- 请求体参数说明：

| 参数名 |解释 |
| ----  | --- |
| token | 登录凭据 |

- 响应示例：空响应

### 注册信息查重检测（检测学号、用户名是否重复）

- 请求url： `/register/checking/{userNumber}/{username}`
- 请求方法：`get`
- **URL**参数说明：

| 参数名 |解释 |
| ----  | --- |
| userNumber | 学号 |
| username | 用户名 |

- 请求体参数说明：

| 参数名 |解释 |
| ----  | --- |
| token | 登录凭据 |

- 响应示例：空响应

### 注册信息查重检测（检测身份证、邮箱、手机号是否重复）

- 请求url： `/register/checking`
- 请求方法：`post`
- 请求体参数说明：

| 参数名 |解释 |
| ----  | --- |
| idNumber | 身份证号 |
| email | 邮箱 |
| phone | 手机号码 | 

- 响应示例：空响应

### 注册 *（待测试/补充）*

- 请求url： `/register`
- 请求方法：`post`
- 请求体参数说明：

| 参数名 |解释 |
| ----  | --- |
| userNumber | 学号 |
| nickname | 用户名 |
| phone | 手机号码 | 
| verifyCode | 验证码 |
| password | 密码 |
| email | 邮箱 |
| idNumber | 身份证号 |

- 响应示例：*（待测试/补充）*

### 获取当前学期信息

- 请求url： `/semester`
- 请求方法：`get`
- 请求体参数说明：

| 参数名 |解释 |
| ----  | --- |
| token | 登录凭据 |

- 响应示例：
```json
{
    "error_code": 0,
    "message": "成功",
    "result": {
        "semesterName": "24253",
        "semesterStartAt": "2025-09-08",
        "semesterStartTimestamp": 1757260800
    }
}
```
- 响应参数说明：

| 参数名 |解释 |
| ----  | --- |
| semesterName | 学期名（似乎与实际并不一致，如文档编写时为25261学期）|
| semesterStartAt | 该学期开始日期 | 
| semesterStartTimestamp | 该学期开始时间戳 |

### **永久**注销账号
- 请求url： `/auth/logout`
- 请求方法：`post`
- 请求体参数说明：

| 参数名 |解释 |
| ----  | --- |
| token | 旧token |

- 响应示例：空响应

### 获取cid（消息推送使用）（待补充）

### 错误码
| 错误码 | 解释 |
| --- | --- | 
| 0 | 成功 | 
| 40001 | 未知错误，请联系管理员 |
| 40002 | 该用户不存在 | 
| 40003 | 未知错误，请联系管理员 |
| 40004 | 用户名或密码错误 |
| 40005 | 登录失效，请重新登录 |
| 50001 | 数据库错误 |
| 50002 | 逻辑错误或数据库错误 |
| 50003 | 未绑定的url，请联系管理员 |
| 50004 | 错误的app_key或app_secret |
| 50005 | 学号和身份证号不匹配 |
| 50006 | 用户名和邮箱已存在 |
| 50007 | 用户名已存在 |
| 50008 | 邮箱已存在 |
| 50009 | 手机号码无效 |
| 50010 | 未知错误，请联系管理员 |
| 50011 | 验证失败，请重新尝试 |
| 50012 | 电子邮件或手机号格式不规范 |
| 50013 | 电子邮件或手机号重复 |
| 50014 | 手机号已存在 |
| 50015 | 升级失败，目标升级账号信息不存在 |
| 50016 | 无此学院 |
| 50019 | 用户名中含有非法字符 |
| 50020 | 用户名过长 |
| 50021 | 该学号所属用户已注册过 |
| 50022 | 该身份证号未在系统中登记 |
