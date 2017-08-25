## 主要思路

```html
<input type="file" id="choose" accept="image/*">
```

* 使用fileReader读取图片，转化成datauri也就是base64的形式
* canvas利用drawimage api渲染图片到canvas画布之中
* canvas利用data.toDataUrl进行压缩上传

## 中间遇到的坑儿

### 读取图片问题

* 图片旋转的问题，ios拍照图片会旋转，利用exif获取元信息，并进行转化

  ```
  原因：
  照片有自己的元信息，所以会带着元信息显示，实际上竖着拍的照片添加了旋转的信息，那为什么经过canvas之后就不一样了呢，ios本身拍照使用的是竖着拍的照片增加了一个顺时针的角度旋转，当对其进行canvas显示之后将这些信息丢失了，因此需要在之前对其进行解析，保存下来方向，再使用canvas进行方向转换
  ```

* IOS图片过大会出现压扁的情况

  ```

  ```

* canvas显示图片模糊的问题

  ```

  ```


### 上传问题

* 可以直接把Base64的字符串上传到服务器，然后由服务端解码为JPG图片

* 也可以在前端解码上传。如果要在前端解码并以文件方式上传，先要用[atob函数](https://developer.mozilla.org/zh-CN/docs/Web/API/WindowBase64/atob)把Base64解开，然后转换为[ArrayBuffer](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/ArrayBuffer)，再用它创建一个[Blob对象](https://developer.mozilla.org/zh-CN/docs/Web/API/Blob)。文件方式上传需要用`multipart/form-data`格式，可以利用[FormData对象](https://developer.mozilla.org/zh-CN/docs/Web/Guide/Using_FormData_Objects)组装生成好的Blob对象来实现。


  ```javascript
  var ndata = compress(img);
    ndata = window.atob(ndata); //将base64格式的数据进行解码
    
    //新建一个buffer对象，用以存储图片数据
    var buffer = new s(ndata.length);
    for(var i = 0; i < text.length; i++) {
        buffer[i] = ndata.charCodeAt(i);
    }
    
    //将buffer对象转化为Blob数据类型
    var blob = getBlob([buffer]);
    
    var fd = new FormData(),
        xhr = new XMLHttpRequest();
    fd.append('file', blob);
    
    xhr.open('post', url);
    xhr.onreadystatechange = function() {
        //do something
    }
    xhr.send(fd);
  ```
  formdata
  ```

* 服务器端需要进行一些验证以免被攻击


  ```
关于blob，arraybuffer，formdata对象

## filereader

readAsDataURL 读取到img文件的base64格式，传送个img，这样才可以渲染到canvas上，从而进行压缩操作。

## Canvas drawImage（）

其中的image参数为

`image` 绘制到上下文的元素。允许任何的 canvas 图像源([`CanvasImageSource`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasImageSource))，例如：[`HTMLImageElement`](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLImageElement)，[`HTMLVideoElement`](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLVideoElement)，或者 [`HTMLCanvasElement`](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLCanvasElement)。

## Image

```javascript
var img1 = new Image(); // HTML5 构造器
img1.src = 'image1.png';
img1.alt = 'alt';
document.body.appendChild(img1);

var img2 = document.createElement('img'); // 使用 DOM HTMLImageElement
img2.src = 'image2.jpg';
img2.alt = 'alt text';
document.body.appendChild(img2);

// 使用文档中的第一个img
alert(document.images[0].src);
```

## blob



## arraybuffer

## formdata

