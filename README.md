# UniLoop 多校联结加载页

一个单文件、可独立运行的 UniLoop 全屏加载动画。页面以五所悉尼高校的校徽为节点，沿摩天轮式环轨缓慢转动，并随着真实加载进度逐段绘制圆角五角星连接线。

## 页面效果

- 中央 UniLoop 地球标识保持固定，周围是 University of Sydney、UNSW、UTS、Western Sydney University 和 Macquarie 五个校徽。
- 五枚校徽围绕同一个固定中心匀速旋转，校徽自身始终正立。
- 五角星按 `USYD → UNSW → Western Sydney → Macquarie → UTS` 的次序连接，路径带圆角并随进度绘制。
- 节点轮流轻微放大、发光；背景保留悉尼歌剧院剪影与简约波浪线。
- `LOADING`、百分比和进度条同步更新；完成后淡出。
- 页面为浅灰白背景，无第三方 JS/CSS 依赖。

## 直接预览

保持 `index.html` 与 `assets/` 的相对目录结构，直接打开 `index.html` 即可预览。也可将整个目录部署到 GitHub Pages 或其他静态托管服务。

## 接入真实加载进度

宿主网页可调用全局方法：

```js
window.setUniLoopLoadingProgress(38);
window.setUniLoopLoadingProgress(100);
```

也可发出自定义事件：

```js
window.dispatchEvent(new CustomEvent("uniloop:loading-progress", {
  detail: { progress: 38 }
}));
```

进度范围为 `0–100`。接入外部进度后，演示进度自动停止；进度到 100 且校徽资源完成加载后，遮罩淡出。独立预览时，演示进度约 3 秒走完；加载页至少显示约 1.4 秒，并有 10 秒超时兜底。

## 动画参数

参数集中在 `index.html` 内联脚本开头：

| 参数 | 当前值 | 用途 |
| --- | ---: | --- |
| `WHEEL_PERIOD` | 24,000 ms | 校徽环绕一周的时长 |
| `DEMO_LOAD_DURATION` | 3,000 ms | 独立预览的模拟进度时长 |
| `MIN_DISPLAY_DURATION` | 1,400 ms | 最短显示时长，避免快速加载时闪一下 |
| `COMPLETION_FADE_DELAY` | 450 ms | 到达 100% 后的淡出缓冲 |
| `FALLBACK_TIMEOUT` | 10,000 ms | 防止宿主进度没有回调导致遮罩卡住 |

## 文件

- `index.html`：完整 HTML、CSS、Canvas 绘图与原生 JavaScript 动画。
- `assets/`：五所大学的徽章或标志图片。
- `docs/ITERATION_HISTORY.md`：需求变化、设计取舍和实现过程记录。
- `docs/INTEGRATION.md`：集成步骤与进度事件示例。

## 素材与商标

校徽和大学标志属于各学校或原始权利人。仓库内素材用于本项目演示和集成；公开部署、商业使用或展示学校名称/标志前，请确认各校品牌和商标使用要求。Western Sydney University 校徽图像由项目委托方提供。

## 项目状态

这是可以交给开发组继续接入的前端交付物。真实网站接入时，需由宿主页面在加载状态变化时更新进度；若当前系统不能提供准确百分比，应使用阶段性进度或不确定进度策略，避免展示虚假的“真实进度”。
