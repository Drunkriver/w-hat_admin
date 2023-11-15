
技术选型
Vite2 + TypeScript + Vue3 + element-plu

配置后端跨域
后端java项目配置跨域参数。 在Spring Boot轻论坛项目里找到crossOrigin.properties文件，将前端的访问地址写入allowedOrigins属性里面。修改完需要重启项目才能生效
服务器部署时使用Nginx配置跨域

推荐使用node 16及以上版本。

将本项目源代码下载到您的本地硬盘里，解压出来，进入您的项目目录，打开配置文件.env.development和.env.production，将VITE_API_URL属性值修改为后端访问地址。
然后执行命令

安装
npm install

运行
npm run dev

打包
npm run build
