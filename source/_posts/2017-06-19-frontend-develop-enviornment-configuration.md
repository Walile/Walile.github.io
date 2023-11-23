---
layout: post
title: 前端設定專案livereload (利用gulp)
date: 2017-06-19 20:12:34
tags: frontend
---
因為開發需要，希望可以自動監控專案底下的HTML/CSS/JavaScript，只要有變更即更新瀏覽器，就不用手動自己重新整理了。而且寫Less的時候，也可以在存檔的時候自動compile成css使用，很方便的處理方式~ 
<!--more-->
接下來就開始把環境設定一下：

1. 初始化專案並且建立package.json
	<code>npm init --yes</code>
  
2. 建立node.js的虛擬環境：(先請安裝nodeenv)
  <code>nodeenv .env</code>
  
  後面的.env可以置換成想要置放的資料夾名稱
  
3. 將環境換成剛剛建好的node.js虛擬環境(此做法在於如果不小心出了問題，比較不會影響到整個電腦的node.js環境
   <code>source .env/bin/actiave</code>
   
   到時候有需要退出環境的時候，只需要直接執行
   <code>deactivate_node</code>
   
   就可以退出環境了。
   
4. 安裝所需要的套件：
   這個安裝的目的是希望能夠監控專案裡面的HTML/LESS/JavaScript檔案，有變更的時候就會編譯LESS或是重新整理正在開發的瀏覽器(即Livereload的功能)，基本上來說，需要先利用npm安裝以下幾個套件：
   
- [gulp](http://gulpjs.com/) 這邊個人會使用gulp來處理，其實也可以使用[grunt](https://gruntjs.com/)，但因為沒玩過就是了。(記得要安裝在global)
- gulp-cli (在terminal使用gulp，記得要裝在global)
- gulp-less
- [browser-sync](https://www.browsersync.io/) (檔案改變的時候可以將結果同步瀏覽器，不用再另外安裝extension)

接下來就示專案需求可以安裝webpack或是babel之類的工具了。

5. 設定gulpfile.js
   因為gulp吃的是gulpfile.js，請先在專案根目錄中設定gulpfile.js，接下來準備設定gulpfile.js的部分：

##### 這邊是先在gulpfile.js上設定載入使用的library
<code>var gulp = require("gulp");
var less = require("gulp-less");
var browsersync = require("browser-sync").create();
</code>

##### 下方的部分是設定less file compile
<code>gulp.task('less',function(){
    gulp.src('./less/*.less')
    .pipe(less())
    .pipe(gulp.dest('./css/'))
});</code>


##### 設定監控檔案，有變動即利用rowsersync重新整理瀏髡器
<code>gulp.task('watch',function(){
    gulp.watch('./less/*.less',['less']).on("change", browsersync.reload)
    gulp.watch('./js/*.js').on("change", browsersync.reload)
    gulp.watch("./*.html").on("change", browsersync.reload)
});</code>

###### browser sync configuration
<code>gulp.task('browser-sync', function(){
    browsersync.init({
        server: {
            baseDir: "./",
            proxy: "test.dev"
        },
        ui: {
            port: 3000,
            weinre:{
                port: 3001
            }
        }
    })
})</code>

###### 這個是最後一行需要放上的部分，default startup的設定
<code>gulp.task('default',['less','watch', 'browser-sync']);</code>

以上檔案處理完畢後，就可以回到專案資料夾根目錄的地方，直接用<code>gulp</code>即可以執行我們設定好的環境並且進行開發了。

