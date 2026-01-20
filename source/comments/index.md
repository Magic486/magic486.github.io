---
title: 留言板
date: 2025-03-03 22:00:00
type: "comments"
---

<!-- 留言板顶部定制代码 -->
<style>
/* 二次元浅蓝主色调 */
#page {
  background: linear-gradient(150deg,rgb(252, 252, 252) 20%, #f0faff 80%);
  padding: 2rem !important;
  border-radius: 15px;
  box-shadow: 0 8px 20px rgba(16, 59, 95, 0.15);
  position: relative;
  overflow: hidden;
}



/* 标题样式 */
.board-title {
  font-family: 'Ma Shan Zheng', cursive;
  color: #2196F3;
  text-shadow: 2px 2px 0px #BBDEFB;
  font-size: 2.5rem;
  border-bottom: 3px dashed #90CAF9;
  padding-bottom: 1rem;
  position: relative;
  z-index: 2;
}



/* 装饰图标 */
.decor-icon {
  position: absolute;
  width: 80px;
  opacity: 0.8;
  transition: transform 0.3s ease;
}

.decor-icon.left {
  left: 20px;
  top: 50px;
}

.decor-icon.right {
  right: 20px;
  top: 40px;
}
</style>

<div class="board-header">
  
  <h1 class="board-title"> 有什么想说的...</h1>
  
  
  <div class="welcome-text">
    <p> ✨ 好吃的<br>
    ✨ 好玩的 <br>   
    ✨ 都来说说吧       </p>
  </div>
</div>
