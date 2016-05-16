#webpack和gulp在多页面自动化构建中的应用#
最近在学习webpack，为了巩固所学，所以准备将自己学习和实践的过程记录下来。这篇blog主要是记录用webpack和gulp构建多页面的自动化工程。

webpack支持AMD，commonjs规范，能够充当模块加载器和模块打包器，而且在webpack看来，除了js之外，其他的img，css等都是资源，都可以进行管理，因此webpack的强大可想而之，而这也使他的学习成本要比gulp高很多。在此多页面的自动化构建过程中，我将用webpack来管理js，其他的图片，样式都由gulp来管理。我将从开发和发布两个阶段来进行构建。

##开发阶段
每次学习一个新的框架或者工具的时候，想做demo但总是感觉无从下手，就我自己而言应该是对需求的不明确导致的，不知道该用这些工具去完成哪些任务，所以我以需求为导向，用这些工具来实现这些需求。

在开发阶段，一般的需求分为这几类：

1. 预处理器语言编译
2. 静态资源的管理
3. 本地服务器模拟

从这个3个需求点出发，用webpack和gulp构建开发环境。  

####预处理器语言编译

由于我目前没有接触过js的预处理器语言，所以在项目中使用js进行开发，样式表使用预处理器语言sass，但就编译过程来说，js预处理器语言的编译应该和sass的编译大同小异。  
所需插件:  
gulp-sass  
代码(gulpfile.js):   

    var gulpsass =require('gulp-sass');
    gulp.task('sass', function() {
        gulp.src('assets/sass/**/*.scss')  
            .pipe(gulpsass().on('error', gulpsass.logError))
            .pipe(gulp.dest('assets/css'));
    });

执行上述任务，将把assets/sass 目录下的sass文件编译成css文件，并放在assets/css 目录下。例如：assets/sass/index/index.scss -->  assets/css/index/index.css

在开发过程中，我们经常会更改sass文件，如果每次修改都需要手动去完成sass的编译工作未免太低效了，所以需要对这些sass文件进行监听，只要发生改变就自动完成编译工作。这里使用gulp提供的watch方法完成这一任务。

    gulp.watch('assets/sass/**/*.scss', ['sass']);

至此，预处理器的编译任务已经构建完成。

####静态资源的管理与本地服务器模拟

之所以将这两个需求放在一起，是因为静态资源的管理也需要本地服务器充当静态服务器。因为在此构建中，js是由webpack来进行管理，所以静态服务器需要两类：1.node的中间件express来充当除了js之外的静态服务器 2. webpack-dev-server来充当js静态服务器。

#####express

express中间件是开发过程中是一个重要的工具，一方面他充当静态服务器，管理出js之外的静态资源，一方面他充当了后台服务器的角色，处理客户端的请求，并将mock数据返回给客户端。想了解express的api文档可以移步[Express 4.x API 中文手册](http://www.expressjs.com.cn/4x/api.html)。

express所要承担的任务我们也将其注册在gulpfile.js中，我们将一步步完成该任务的构建。我们将在基本代码的基础上，一步步地往里面叠加代码。

基本代码：

    //启动服务的时候应该确保预处理器语言已经编译，所以该任务需要依赖sass的编译任务
    var express = require('express');
    var path = require('path'); 
    gulp.task('server', ['sass'], function() {  
        var app = express();
    });

至此我们就能够使用这个app来帮我们做些事情。

1.需要能够处理html请求，返回相应的html 2.充当静态服务器 3.处理ajax异步请求 现在将这3个任务迭代到之前的基本代码中。

    gulp.task('server', ['sass'], function() {
        var app = express();
        //处理html的请求
        app.use('/', function(req, res, next) {
            //req是http.ServerResponse的实例，res是http.IncomingMessage的实例
            //express 对这两个实例进行了分装

            //http://www.aa.bb.com/path/demo.html?a=b --> originalUrl=/path/demo.html?a=b;path=/path/demo.html
            var requestUrl = req.path;
            if (requestUrl === '/') {
                //如果访问网站根目录则给出默认的首页
            } else if (/.+\.html$/.test(requestUrl)) {
                //如果是html，则根据path返回相应的html
            } else {
                next();
            }
        });
        //至此，html的请求处理完成
        
        //处理ajax异步请求
        app.use('/', function(req, res, next) {
            var path = req.path;
            if (/.+\.do$/.test(path)) {
                //根据path返回mock好的数据
            } else {
                next();
            }
        });
        //ajax请求处理结束

        //静态资源的请求
        //因为此构建中，js是用webpack来管理的，所以对于js的请求，这里将通过request  node中间件转发请求到webpack-dev-server,其他静态资源则通过express.static来进行管理


        //处理js  为了符合人类对静态资源的引用思维，在html直接通过'/js/**/*.js'引用js
        app.use('/js', function(req, res, next) {
            var path = req.path;
            if (/.+\.js/.test(path)) {
                //通过request转发请求到webpack－dev－server:
                var requestUrl = `http://{host}:{port}/js/{path}`;
                var request = require('request');
                request(requestUrl).pipe(res);
            }
        });
        //js处理完毕


    });