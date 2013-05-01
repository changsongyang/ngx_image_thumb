# Nginx Image 模块 
##### QQ:3078825
##### 邮箱:3078825#qq.com


### 模块同时支持 Nginx 和 tengine

-本nginx模块主要功能是对请求的图片进行缩略/水印处理，支持文字水印和图片水印。
-支持自定义字体，文字大小，水印透明度，水印位置。
-判断原图是否是否大于指定尺寸才处理。
....等等


## 编译方法 

编译前请确认您的系统已经安装了libcurl-dev  libgd2-dev  libpcre-dev 依赖库

### Debian / Ubuntu 系统举例
```bash
# 如果你没有安装GCC相关环境才需要执行
$sudo apt-get install build-essential m4 autoconf automake make 
$sudo apt-get install libgd2-noxpm-dev libcurl4-openssl-dev libpcre3-dev
```

### CentOS /RedHat / Fedora
```bash
# 请确保已经安装了gcc automake autoconf m4 
$sudo yum install gd-devel pcre-devel libcurl-devel 

```

### FreeBSD / NetBSD / OpenBSD
```

# 不多说了，自己用port 把libcurl-dev libgd2-dev libpcre-dev 装上吧
# 编译前请确保已经安装gcc automake autoconf m4 
```

### Windows
```
# 也支持的，不过要修改的代码太多了，包括Nginx本身，用VC++来编译
# 嫌麻烦可以用cygwin来编译。还是不建议你这么做了，用Unix/Linux操作系统吧。
```

###下载nginx / tengine 源代码

#### 然后下载本模块代码，并放置在nginx源代码的nginx_image_module目录下
#### 选Nginx还是Tengine,您自己看,两者选其一

```bash
# 下载Tengine
$wget http://tengine.taobao.org/download/tengine-1.4.5.tar.gz
$tar -zxvf tengine-1.4.5.tar.gz
$cd 解压后的目录
```

```bash
# 下载Nginx
$wget http://nginx.org/download/nginx-1.4.0.tar.gz
$tar -zxvf nginx-1.4.0.tar.gz
$cd 解压后的目录
```

```bash
$mkdir ./nginx_image_module
$cd ./nginx_image_module
$wget https://github.com/3078825/nginx-image/archive/master.zip
$unzip master.zip
$cd ..
$./configure --add-module=./nginx_image_module
$make
$sudo make install 
```

## 配置方法
```bash
vim `nginx.conf` 
```

在
```apache
location / {
   root html;
   #添加以下配置
   image on;
   image_output on;
}
```

或者指定目录开启 
```
location /upload {
   root html; 
   image on;
   image_output on;
}
```


## 其他参数说明：
```
image on/off 是否开启缩略图功能,默认关闭

image_backend on/off 是否开启镜像服务，当开启该功能时，请求目录不存在的图片（判断原图），将自动从镜像服务器地址下载原图

image_backend_server 镜像服务器地址

image_output on/off 是否不生成图片而直接处理后输出 默认off

image_jpeg_quality 75 生成JPEG图片的质量 默认值75

image_water on/off 是否开启水印功能

image_water_type 0/1 水印类型 0:图片水印 1:文字水印

image_water_min 300 300 图片宽度 300 高度 300 的情况才添加水印

image_water_pos 0-9 水印位置 默认值9 0为随机位置,1为顶端居左,2为顶端居中,3为顶端居右,4为中部居左,5为中部居中,6为中部居右,7为底端居左,8为底端居中,9为底端居右

image_water_file 水印文件(jpg/png/gif),绝对路径或者相对路径的水印图片

image_water_transparent 水印透明度,默认20

image_water_text 水印文字 "Power By Vampire"

image_water_font_size 水印大小 默认 5

image_water_font;//文字水印字体文件路径

image_water_color 水印文字颜色,默认 #000000
```

## 调用说明

这里假设你的nginx 访问地址为 http://127.0.0.1/

并在nginx网站根目录存在一个 test.jpg的图片

通过访问 

http://127.0.0.1/test.jpg!c300x200.jpg 将会 生成/输出 `test.jpg` **300x200**的缩略图

其中**c**是生成图片缩略图的参数，**300**是生成缩略图的宽 **200**是生成缩略图的高

一共可以生成四种类型的缩略图。

```
**c**参数按请求宽高比例从图片高度*10%*处开始截取图片，然后缩放/放大到指定尺寸（**图片缩略图大小等于请求的宽高**）

**m**参数按请求宽高比例居中截图图片，然后缩放/放大到指定尺寸（**图片缩略图大小等于请求的宽高**）

**t**参数按请求宽高比例按比例缩放/放大到指定尺寸（**图片缩略图大小可能小于请求的宽高**)

**w**参数按请求宽高比例缩放/放大到指定尺寸，空白处填充白色背景颜色（**图片缩略图大小等于请求的宽高**）
```

 
## 调用举例

- http://127.0.0.1/test.jpg!c300x300.jpg

- http://127.0.0.1/test.jpg!t300x300.jpg

- http://127.0.0.1/test.jpg!m300x300.jpg

- http://127.0.0.1/test.jpg!w300x300.jpg

- http://127.0.0.1/test.c300x300.jpg

- http://127.0.0.1/test.t300x300.jpg

- http://127.0.0.1/test.m300x300.jpg

- http://127.0.0.1/test.w300x300.jpg





