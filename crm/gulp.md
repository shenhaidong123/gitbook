# Gulp

#### 作者：杨柠瑞

#### 邮箱：yangnr@haomo-studio.com



##1.简介
gulp是前端开发过程中对代码进行构建的工具，不仅能对网站资源进行优化，而且在开发过程中很多重复的任务能够使用正确的工具自动完成，从而提高我们的工作效率。

##2.安装gulp
1. 首先我们要全局安装：  

		npm install --global gulp

2. 作为项目的开发依赖，我们要进去到项目的根目录安装：  

		npm install --save-dev gulp

3. 在项目根目录下创建一个名为 gulpfile.js 的文件：

		var gulp = require('gulp');
		
		gulp.task('default', function() {			// 默认执行的代码
		
		}); 
		 
##3.使用gulp插件  
> 这里以代码压缩的插件为例  

1. 首先，安装我们所需的插件：  

		npm install gulp-uglify
		
2. 在 gulpfile.js 中加载插件  

		var gulp = require('gulp'),
		uglify = require('gulp-uglify');
3. 建立任务

		var gulp = require('gulp'),
			uglify = require('gulp-uglify');  
			
		gulp.task('default',['scripts']);
		
		gulp.task('scripts', function() {
			return gulp.src('app/scripts/**')
    			.pipe(uglify())
    			.pipe(gulp.dest('dist/assets/js'))
		});
4. 执行任务  
	在命令行输入 gulp scripts 执行任务，如果加入默认执行 直接输入 gulp 执行

> gulp.task();	用来创建任务  
> gulp.src()	需要处理的文件的路径，可以是多个文件以数组的形式，也可以是正则表达式  
> .pipe()	将需要处理的文件导向插件  
> gulp.dest()		设置生成文件的路径


##常用插件
sass的编译（gulp-ruby-sass）  
压缩css（gulp-minify-css）   
js代码校验（gulp-jshint）  
合并js文件（gulp-concat）  
压缩js代码（gulp-uglify）  
压缩图片（gulp-imagemin）  

	