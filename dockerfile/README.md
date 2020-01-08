# dockerfile 学习笔记
## 具体用法参考
-[Docker Docs](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/)

## 什么是dockerfile
一个由一系列命令和参数构成的脚本，一个dockerfile里面包含了构建整个 image 的完整命令，可以放在项目目录中，用其构建docker环境从而获得一致的生产环境。

> 比如之前迁移服务器，每个项目基本上有自己的一个独特的环境，有的项目依赖PHP，有的项目依赖Node，有的项目依赖低版本的PHP，这一类的来不及重构但是需要的项目，所以迁移时除了拆分静态资源（图片，视频），在打包时将项目中加入dockerfile，在部署到新的服务器上时，直接运行构建脚本即可

使用起来也很简单：
```bash
$ docker build -t <your username>/image-name
```

## 几个项目环境示例：
### PHP7.0 一个运行 wordpress 的环境
```dockerfile
FROM php:7.0-apache

RUN docker-php-ext-install mysqli pdo pdo_mysql
RUN docker-php-ext-enable mysqli

RUN a2enmod rewrite


ENV APACHE_DOCUMENT_ROOT /var/www/html

RUN sed -ri -e 's!/var/www/html!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/sites-available/*.conf
RUN sed -ri -e 's!/var/www/!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/apache2.conf /etc/apache2/conf-available/*.conf
```

### 一个朴实无华的 Node 环境
```dockerfile
FROM node:12
ENV NODE_ENV=production
ENV HOST 0.0.0.0

# Create app directory
WORKDIR /usr/src/app

EXPOSE 3000
```