> 参考thinkphp3.2.3中Image操作
>
> https://www.kancloud.cn/manual/thinkphp/1878
>
> 不依赖任何框架
>
> 一般情况下都会安装gd库
>
> 需要Imagick请安装 https://pecl.php.net/package/imagick


## 实例化类库

~~~
$image = new \tp5er\Images\Image();
~~~

默认使用GD库进行图像操作，如果需要使用Imagick库操作的话，需要改成：

```
$image = new \tp5er\Images\Image(\Think\Image::IMAGE_IMAGICK); 
// 或者采用
$image = new \tp5er\Images\Image('Imagick');
```

## 图像操作

下面来看下基础的图像操作功能的使用方法。

### 打开图像文件

假设当前入口文件目录下面有一个1.jpg文件

使用open方法打开图像文件进行相关操作：

```
$image = \tp5er\Images\Image(); 
$image->open('./1.jpg');
```

也可以简化成下面的方式：

```
$image = new \tp5er\Images\Image(\tp5er\Images\Image::IMAGE_GD,'./1.jpg'); // GD库
// 或者
$image = new \tp5er\Images\Image(\tp5er\Images\Image::IMAGE_IMAGICK,'./1.jpg');  // imagick库
```

### 获取图像信息

可以获取打开图片的信息，包括图像大小、类型等，例如：

~~~
$width = $image->width(); // 返回图片的宽度
$height = $image->height(); // 返回图片的高度
$type = $image->type(); // 返回图片的类型
$mime = $image->mime(); // 返回图片的mime类型
$size = $image->size(); // 返回图片的尺寸数组 0 图片宽度 1 图片高度
~~~

### 裁剪图片

使用crop和save方法完成裁剪图片功能。

```
//将图片裁剪为400x400并保存为corp.jpg
$image->crop(400, 400)->save('./crop.jpg');
```

支持从某个坐标开始裁剪，例如下面从（100，30）开始裁剪：

```
//将图片裁剪为400x400并保存为corp.jpg
$image->crop(400, 400,100,30)->save('./crop.jpg');
```

### 生成缩略图

使用thumb方法生成缩略图

~~~
// 按照原图的比例生成一个最大为150*150的缩略图并保存为thumb.jpg
$image->thumb(150, 150)->save('./thumb.jpg');
~~~

可以支持其他类型的缩略图生成，设置包括`\tp5er\Images\Image`的下列常量或者对应的数字：

~~~
IMAGE_THUMB_SCALE     =   1 ; //等比例缩放类型
IMAGE_THUMB_FILLED    =   2 ; //缩放后填充类型
IMAGE_THUMB_CENTER    =   3 ; //居中裁剪类型
IMAGE_THUMB_NORTHWEST =   4 ; //左上角裁剪类型
IMAGE_THUMB_SOUTHEAST =   5 ; //右下角裁剪类型
IMAGE_THUMB_FIXED     =   6 ; //固定尺寸缩放类型
~~~

#### 居中裁剪

```
// 生成一个居中裁剪为150*150的缩略图并保存为thumb.jpg
$image->thumb(150, 150,\tp5er\Images\Image::IMAGE_THUMB_CENTER)->save('./thumb.jpg');
```

#### 左上角剪裁

```
$image->thumb(150, 150,\tp5er\Images\Image::IMAGE_THUMB_NORTHWEST)->save('./thumb.jpg');
```

#### 缩放填充

```
$image->thumb(150, 150,\tp5er\Images\Image::IMAGE_THUMB_FILLED)->save('./thumb.jpg');
```

#### 固定大小

```
$image->thumb(150, 150,\tp5er\Images\Image::IMAGE_THUMB_FIXED)->save('./thumb.jpg');
```

### 添加图片水印

```
//将图片裁剪为440x440并保存为corp.jpg
$image->crop(440, 440)->save('./crop.jpg');
// 给裁剪后的图片添加图片水印（水印文件位于./logo.png），位置为右下角，保存为water.gif
$image->water('./logo.png')->save("water.gif");
// 给原图添加水印并保存为water_o.gif（需要重新打开原图）
$image->open('./1.jpg')->water('./logo.png')->save("water_o.gif"); 
```

water方法的第二个参数表示水印的位置，可以传入下列Think\Imag类的常量或者对应的数字：

```
IMAGE_WATER_NORTHWEST =   1 ; //左上角水印
IMAGE_WATER_NORTH     =   2 ; //上居中水印
IMAGE_WATER_NORTHEAST =   3 ; //右上角水印
IMAGE_WATER_WEST      =   4 ; //左居中水印
IMAGE_WATER_CENTER    =   5 ; //居中水印
IMAGE_WATER_EAST      =   6 ; //右居中水印
IMAGE_WATER_SOUTHWEST =   7 ; //左下角水印
IMAGE_WATER_SOUTH     =   8 ; //下居中水印
IMAGE_WATER_SOUTHEAST =   9 ; //右下角水印
```

例如：

```
$image->open('./1.jpg')->water('./logo.png',\tp5er\Images\Image::IMAGE_WATER_NORTHWEST)->save("water.jpg"); 
```

还可以支持水印图片的透明度（0~100，默认值是80），例如：

```
$image->open('./1.jpg')->water('./logo.png',\tp5er\Images\Image::IMAGE_WATER_NORTHWEST,50)->save("water.jpg"); 
```

也可以支持给图片添加文字水印（假设在入口文件的同级目录下存在1.ttf字体文件），例如：

```
$image->open('./1.jpg')->text('ThinkPHP','./1.ttf',20,'#000000',\tp5er\Images\Image::IMAGE_WATER_SOUTHEAST)->save("new.jpg"); 
```
