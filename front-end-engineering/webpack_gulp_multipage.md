#webpack和gulp在多页面自动化构建中的应用#
最近在学习webpack，为了巩固所学，所以准备将自己学习和实践的过程记录下来。这篇blog主要是记录用webpack和gulp构建多页面的自动化工程。

webpack支持AMD，commonjs规范，能够充当模块加载器和模块打包器，而且在webpack看来，除了js之外，其他的img，css等都是资源，都可以进行管理，因此webpack的强大可想而之，而这也使他的学习成本要比gulp高很多。在此多页面的自动化构建过程中，我将用webpack来管理js，其他的图片，样式都由gulp来管理。我将从开发和发布两个阶段来进行构建。

##开发阶段