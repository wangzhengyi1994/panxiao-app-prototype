# 磐销云输入法 高保真 HTML 原型 - 真实CSS版

## 核心要求
不要用截图贴图！每个页面用真实 HTML/CSS 还原，输入框可输入，按钮可点击，视觉严格还原Figma。
只有无法用CSS还原的图形素材（吉祥物、3D人物）才用图片。

## 可用素材（assets/目录）
- mascot.png — 启动页3D獾吉祥物（戴眼镜，从底部探出）
- login_character.png — 登录/注册/推荐页Header右侧3D商务人物
- recommend_character.png — 推荐页人物（备用）
- home.png — 首页（含hero插图，可用作首页背景参考）
- ai_func.png / bangni_shuo.png / bangni_hui.png / popular_moments.png — 键盘AI面板各Tab

## 颜色规范
```
主蓝: #3D6EF5
深蓝渐变Header: #1B6EF3 → #1558D6
表单背景: #F5F6FA
激活边框: #3D6EF5，2px
文字主色: #1A1A1A
次文字: #666666
占位文字: #AAAAAA / #BBBBBB
图标灰: #999999
分割线: #F0F0F0
背景灰: #F5F5F5
```

## 尺寸规范
```
屏幕宽度: 375px（居中显示）
水平内边距: 20px
输入框: height 52px, border-radius 12px
按钮: height 52px, border-radius 14px
白色卡片顶部圆角: 24px
```

## 字体
```css
font-family: -apple-system, 'PingFang SC', 'SF Pro Display', system-ui, sans-serif;
```

## 移动端适配
```css
<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no">
@media (max-width: 430px) { .phone { width: 100%; } }
```

## 页面1: index.html（启动页）
```
全屏背景 #3D6EF5
状态栏（CSS模拟：时间9:41 + 图标）
中部文案（距顶35%）:
  "每一句对话，" 第一行
  "都在走向成交" 第二行
  白色，26px，regular，居中，line-height 1.6

底部吉祥物区域:
  <img src="assets/mascot.png" style="position:absolute;bottom:0;left:50%;transform:translateX(-50%);width:100%;max-width:375px">

2秒后自动跳转login.html
```

## 页面2: login.html（登录页）

### 顶部Header（高220px，蓝色）
```css
background: #3D6EF5;
border-radius: 0 0 24px 24px;
position: relative; overflow: hidden;
```
- 左侧（Header左下区域）：白色气泡卡片
  - border-radius: 12px 12px 12px 0（左下角无圆角，有气泡尖角）
  - 内容："欢迎使用"（#333，16px）+ "磐销云输入法"（#3D6EF5，16px，bold）
  - box-shadow: 0 4px 12px rgba(0,0,0,0.1)
  - 气泡尖角：CSS before伪元素，底部左侧三角
- 右侧：3D人物图片（assets/login_character.png），绝对定位右下角，高约180px

### 表单区域（白色，padding 0 20px）
- 各字段间距12px
- 输入框组件：
  ```html
  <div class="input-wrap">
    <span class="input-icon"><!-- SVG图标 --></span>
    <input type="text" placeholder="请输入账号">
    <!-- 密码框额外加眼睛图标按钮 -->
    <!-- 验证码框额外加"获取验证码"文字按钮 -->
  </div>
  ```
  ```css
  .input-wrap { display:flex; align-items:center; background:#F5F6FA; border-radius:12px; height:52px; padding: 0 14px; margin-bottom:12px; border: 2px solid transparent; }
  .input-wrap:focus-within { background:#FFF; border-color:#3D6EF5; }
  input { flex:1; border:none; outline:none; background:transparent; font-size:15px; color:#1A1A1A; }
  input::placeholder { color:#BBBBBB; }
  .input-icon { width:18px; height:18px; margin-right:10px; color:#999; flex-shrink:0; }
  ```

- 账号输入框（身份证SVG图标）
- 密码输入框（锁SVG图标 + 右侧眼睛切换明文/密文）
- 验证码输入框（盾牌SVG图标 + 右侧"获取验证码"按钮，点击倒计时60秒）

- 登录按钮：
  ```css
  .btn-primary { width:100%; height:52px; background:#3D6EF5; border:none; border-radius:14px; color:#FFF; font-size:18px; font-weight:500; cursor:pointer; margin-top:20px; }
  .btn-primary:active { opacity:0.85; }
  ```

- 注册引导（按钮下方16px）：
  ```html
  <p class="link-row">没有账号？<a href="register.html">去注册</a></p>
  ```
  灰色+蓝色链接，14px，居中

- 底部协议（页面最底部，固定）：
  ```html
  <p class="agreement">登录即代表您已并同意<a>《用户服务协议》</a>和<a>《隐私政策》</a></p>
  ```
  12px，灰色，居中

## 页面3: register.html（注册页）
与login.html结构相同，差异：
- 4个输入框（账号/验证码/密码/确认密码）
- 按钮文字"注册"，点击→ recommend1.html
- 底部"已有账号？去登录"→ login.html

## 页面4: recommend1.html（选行业）

### 顶部Header（高220px，渐变）
```css
background: linear-gradient(180deg, #1B6EF3, #1558D6);
border-radius: 0 0 30px 30px;
```
- 左上返回箭头（白色SVG）
- 步骤进度条（4段横线，第1段白色，其余rgba(255,255,255,0.4)）
- 左下主标题"选择您的赛道"（白色，24px，bold）+ 副标题"您目前深耕于哪个行业？"（白色，15px）
- 右侧人物图片（assets/login_character.png），绝对定位右下

### 搜索框
```css
.search-wrap { background:#F5F5F7; border-radius:10px; height:44px; margin:16px 20px 0; display:flex; align-items:center; padding:0 12px; }
```

### 行业分类区域（可滚动）
```css
.category-title { font-size:15px; font-weight:600; color:#333; margin:16px 20px 8px; }
.tag-grid { display:grid; grid-template-columns:repeat(3,1fr); gap:8px; padding:0 20px; }
.tag { height:38px; background:#F5F5F7; border-radius:8px; border:1px solid transparent; font-size:14px; color:#333; display:flex; align-items:center; justify-content:center; cursor:pointer; }
.tag.active { background:#DDEEFF; border-color:#1B6EF3; color:#1B6EF3; }
```

行业标签数据（JS生成）：
金融销售: [助贷(默认选中), 个贷, 保险, 理财, 证券, 信用卡]
服务业销售: [家装, 服装, 旅游, 全屋定制, 彩妆, 健身, 美容, 珠宝, 护肤品]
房地产销售: [地产中介, 置业顾问]
教培销售: [招生, 课程, 留学]
汽车销售: [二手车, 车险, 汽车, 汽车配件]
医疗销售: [大健康, 医疗器械, 医药, 口腔, 医美]
广告/会展销售: [媒介广告, 会展会议]
其他销售行业: 右上角有+按钮 + 虚线全宽按钮"都没找到?点击添加您的销售行业"

底部固定按钮"下一步" → recommend2.html

## 页面5: recommend2.html（完善信息）
Header同recommend1（进度条第2段激活）
主标题"属于你的专属个人智能体"，副标题"请完善【个人信息】"

表单（标签+输入框，标签14px #333）：
- 姓名（必填红星*）：单行，占位"例:张明"
- 职位：单行，占位"例:市场总监"
- 个人亮点：多行文本框，height:100px，占位"例:10年快消品行业营销经验..."

底部固定"下一步"→ app.html

## 页面6: app.html（主APP）

### 结构
```
固定顶部导航栏（44px）
可滚动内容区
固定底部TabBar（56px）
```

### 顶部导航栏
```css
background: transparent（首页）/ white（其他）
```
左：App图标（深蓝圆角正方形#1B3A8F，内含白色圆形）+ "磐销云输入法"（16px bold）
右：白色胶囊（border: 1px solid #E8E8E8，padding 6px 12px，border-radius 20px）"👤 电话销售 >"（13px）

### 首页Tab内容（背景渐变 #C5DEFF→#FFFFFF）

Hero区：
- 主标题"懂销售的智能外脑"（32px, bold）
  文字渐变：background: linear-gradient(90deg, #0EA5E9, #2DD4BF); -webkit-background-clip:text; color:transparent
- 副标题胶囊"磐销云输入法 让成交更从容"（深绿背景#00796B，白色13px，圆角20px，padding 6px 16px）
- 使用 assets/home.png 作为hero区域背景图（cover方式）
- "立即体验 →"按钮：background: linear-gradient(135deg, #667EEA, #9B59B6)，height 52px，border-radius 26px，宽80%居中，右上角红色角标"超过7W+同行正在使用"

核心价值卡片（背景 #1A4FBB，圆角16px，mx 16px）：
- 左上青绿标签"输入法核心价值"（#00BFA5背景，白色10px，圆角4px）
- 2×2 Grid，4项：AI赋能销售/效率革命/能力跃迁/成交加速（白色标题+浅色说明）

DeepSeek卡片（#1A4FBB，圆角16px，mx 16px）：
- "已全面接入 DEEPSEEK"（白色20px bold）
- 黄绿圆点+"与DEEPSEEK携手，开启智能新纪元"（白色12px）
- 右侧鲸鱼Emoji 🐳 或SVG（简化表达）

适合人群（4条灰底圆角条，关键词高亮）

### 我的Tab内容
顶部蓝白渐变区：
- 圆形头像（80px，#E0E0E0占位，白色4px边框）
- 手机号"183****5597"（20px bold）
- 有效时间"有效时间:2026-12-31 00:00:00"（12px #999）

白色卡片菜单（mx 16px，圆角16px，shadow）：
5个列表项（图标+文字+右箭头），带分割线：
🚀 检查更新 · 当前版本:1.0.1
🗑 清除缓存
📄 用户服务协议
📖 隐私政策
💬 我的客服

### 底部TabBar（56px，白色，上边框#F0F0F0）
3个Tab：
- 首页（房子SVG，激活蓝色#3D6EF5，未激活#999，点击切换内容区）
- 教程（书本SVG，点击显示空状态提示）
- 我的（人物SVG，点击切换我的内容）

## 页面7: keyboard.html（键盘AI面板）
使用 assets/ai_func.png / bangni_shuo.png / bangni_hui.png / popular_moments.png 直接展示（这些页面复杂度高用截图可以接受），加上Tab切换交互

## 完成后
1. 删除所有旧HTML（index/app/keyboard/login/register/recommend1/recommend2）
2. 重写为上述真实CSS版本
3. git add -A && git commit -m "real css hifi prototype" && git push
4. 运行: openclaw system event --text "高保真CSS原型已完成并推送" --mode now
