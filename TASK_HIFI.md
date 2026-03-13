# 磐销云输入法 高保真 HTML 原型

## 核心方案
每个页面 = Figma 截图（assets/ 目录）作为背景 + 透明热区覆盖层（处理点击跳转）
不写样式还原UI，直接用Figma原图，视觉100%精确

## 文件结构
assets/
  splash.png       启动页
  login.png        登录页
  register.png     注册页
  recommend1.png   选行业
  recommend2.png   完善信息
  home.png         首页
  mine.png         我的
  ai_func.png      AI功能面板
  bangni_shuo.png  帮你说
  bangni_hui.png   帮你回
  popular_moments.png 高赞朋友圈

## 每个HTML页面的结构模板

```html
<!DOCTYPE html>
<html>
<head>
  <meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1">
  <style>
    * { margin: 0; padding: 0; box-sizing: border-box; }
    body { background: #000; display: flex; justify-content: center; min-height: 100vh; align-items: center; }
    .phone-frame {
      position: relative;
      width: 390px;
      max-width: 100vw;
      /* 在移动端时充满全屏 */
    }
    .screen-img {
      width: 100%;
      display: block;
    }
    /* 热区：透明可点击区域 */
    .hotspot {
      position: absolute;
      cursor: pointer;
      /* 默认透明，调试时可加 background: rgba(255,0,0,0.2) */
    }
  </style>
</head>
<body>
  <div class="phone-frame">
    <img class="screen-img" src="assets/XXX.png">
    <!-- 热区覆盖在按钮位置 -->
    <div class="hotspot" style="..." onclick="location.href='next.html'"></div>
  </div>
</body>
</html>
```

## 各页面热区规划

Figma 原始尺寸 375×812（1x）。assets里的PNG是@2x（750×1624px）。
HTML里img宽度390px，所以坐标比例换算：
- x: figma_x * (390/375)
- y: figma_y * (390/375) [近似1.04]

### index.html（启动页）
- 整屏点击热区 → 跳转 login.html
- 3秒后自动跳转（JS setTimeout）

### login.html（登录页）
热区（基于Figma坐标大致估算）：
- 登录按钮: top约64%, left:20px, right:20px, height:52px → login_logic
- "去注册"文字: top约73%, 右侧, 约80px×24px → register.html
- 验证码"获取验证码": top约56%, right:20px, 约90px×30px → JS倒计时

### register.html（注册页）
- 注册按钮 → recommend1.html
- "去登录" → login.html

### recommend1.html（选行业）
- 下一步按钮（底部）→ recommend2.html
- 行业标签：点击切换选中样式（overlay div模拟不了，用JS在图片上画选中框）

### recommend2.html（完善信息）
- 下一步按钮（底部）→ app.html

### app.html（主APP）
背景图根据当前Tab切换：
- 默认显示 home.png
- 底部Tab点击区域：
  - 首页Tab（左1/3，底部64px区域）→ 显示home.png
  - 教程Tab（中1/3）→ 显示home.png（暂用首页）
  - 我的Tab（右1/3）→ 显示mine.png
- 首页"立即体验"按钮 → keyboard.html

### keyboard.html（键盘AI面板）
Tab切换：
- 显示ai_func.png（默认）
- 帮你说 Tab → bangni_shuo.png
- 帮你回 Tab → bangni_hui.png
- 高赞朋友圈 Tab → popular_moments.png
- 键盘工具栏区域点击 → 返回app.html

## 移动端适配
```css
@media (max-width: 430px) {
  .phone-frame { width: 100vw; }
}
```

## 部署
完成后运行：
cd /Users/dang/Documents/maxclaude/panxiao-app-prototype && git add -A && git commit -m "hifi prototype with figma assets" && git push

如果没有远程仓库，先运行：
gh repo create panxiao-app-prototype --public --source=. --remote=origin --push

完成并推送后运行: openclaw system event --text "高保真原型已完成，正在部署" --mode now
