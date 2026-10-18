# 集成指南

## 作为独立加载页面

将 `index.html` 和 `assets/` 一起部署。不可单独移动 HTML 而遗漏图片目录；五枚徽章使用相对路径读取素材。

## 嵌入现有网站

1. 将 `index.html` 中 `.loader` 对应的根容器、`<style>` 和页面底部 `<script>` 移入网站的根布局或首页模板。
2. 将 `assets/` 复制到宿主网站可访问的位置，并按实际目录修改 `<img src>`。
3. 在网站的数据加载流程中调用 `window.setUniLoopLoadingProgress(value)`。
4. 所有关键请求完成后调用 `window.setUniLoopLoadingProgress(100)`。

示例：

```js
function onAppLoadProgress(percent) {
  window.setUniLoopLoadingProgress(percent);
}

async function loadApp() {
  try {
    await loadCriticalData(onAppLoadProgress);
  } finally {
    window.setUniLoopLoadingProgress(100);
  }
}
```

也支持事件接口：

```js
window.dispatchEvent(new CustomEvent("uniloop:loading-progress", {
  detail: { progress: 72 }
}));
```

## 进度数据注意事项

不要用固定计时器伪造真实加载百分比。若底层系统不能计算真实进度，可采用阶段性进度（例如初始化、读取数据、完成），或改为不确定进度样式。当前离线演示中的 3 秒进度仅用于预览。
