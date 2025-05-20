# GCML (*G*roup **C**hat **M**arkup **L**anguage)

## 1. 文档结构
```xml
<!-- 根元素类型 -->
<{group|private}_chat 
  id="QQ群号/对方QQ号" 
  name="群名称/对方昵称" 
  platform="QQ/WeChat/..."> <!-- 平台属性可选 -->

  <!-- 群聊特有结构 -->
  <admin_list>
    <user name="群主名" qq="123456" type="owner"/>
    <user name="管理员名" qq="234567" type="admin"/>
  </admin_list>

  <!-- 消息序列 -->
  <message time="2023-10-01 14:30:00">
    <user name="发送者" qq="345678"/>
    <text>消息内容</text>
    <image url="http://example.com/img.jpg"/>
  </message>
</{group|private}_chat>
```

## 2. 核心元素说明

### 2.1 根元素
- **group_chat** (群聊模式)
  ```xml
  <group_chat id="123456" name="开发者交流群" platform="QQ">
  ```
  
- **private_chat** (私聊模式)
  ```xml
  <private_chat id="987654" name="张三">
  ```

### 2.2 管理员列表 (仅群聊)
```xml
<admin_list>
  <!-- 第一个用户必须是群主 -->
  <user name="李四" qq="100001" type="owner"/>
  <user name="王五" qq="100002" type="admin"/>
</admin_list>
```

### 2.3 用户元素
```xml
<user 
  name="显示名称" 
  qq="用户QQ号" 
  type="owner|admin|norm"/> <!-- type 默认为 norm -->
```

### 2.4 消息元素
```xml
<message time="2023-10-01 15:00:00">
  <user name="发送者" qq="200001"/>
  <!-- 消息组件序列 -->
</message>
```

## 3. 消息组件类型

### 3.1 文本消息
```xml
<text>需要转义的字符使用 &amp; &lt; &gt; 等实体</text>
```

### 3.2 图片消息
```xml
<image url="http://cdn.example.com/1.jpg" format="JPEG">
  <!-- 为图片编码预留 -->
</image>
```

### 3.3 @消息
```xml
<at qq="123456" name="被@用户"/> <!-- name 为可选属性 -->
```

### 3.4 思维链 (COT)
```xml
<think>LLM的推理过程文本</think>
```

## 4. 完整示例

### 群聊示例
```xml
<group_chat id="123456" name="AI开发者群" platform="QQ">
  <admin_list>
    <user name="张伟" qq="10001" type="owner"/>
    <user name="李娜" qq="10002" type="admin"/>
  </admin_list>

  <message time="2023-10-01 09:00:00">
    <user name="王强" qq="20001"/>
    <text>大家看这个示例：</text>
    <image url="http://example.com/sample.png"/>
  </message>

  <message time="2023-10-01 09:01:30">
    <user name="李娜" qq="10002" type="admin"/>
    <at qq="10001" name="张伟"/>
    <text>注意需要转义特殊符号 &amp; &lt; &gt;</text>
  </message>
</group_chat>
```

### 私聊示例
```xml
<private_chat id="987654" name="客服对话">
  <message time="2023-10-01 10:00:00">
    <user name="用户A" qq="30001"/>
    <text>请问如何重置密码？</text>
  </message>

  <message time="2023-10-01 10:00:30">
    <user name="客服机器人" qq="80000"/>
    <think>需要引导用户到密码重置页面</think>
    <text>请访问：example.com/reset</text>
  </message>
</private_chat>
```

## 5. 转义规则
- 使用标准XML实体转义：
  - `&` → `&amp;`
  - `<` → `&lt;`
  - `>` → `&gt;`
  - `"` → `&quot;`
  - `'` → `&apos;`

