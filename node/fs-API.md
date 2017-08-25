### fs.readFIle(,‘utf-8’,function)

### fs.readFileSync(,)同步读取文件

### fs.readdir(,)读取目录

### fs.stat(,function(err, stat){})读取文件或目录的元数据

### fs模块

fs模块允许你通过stream API来对数据进行读写操作。不同是**对内存的分配不是一次完成的**

文件很大的时候，分配内存过大，解决办法一次只读取一块内容，以行尾结束符来切分，再逐块进行分析。

```
fs.readFile('my-file.text', function(err, contents){})
```

上述回调函数需要等到整个文件读取完毕，载入到RAM、可用的情况下触发。

```
var stream = fs.createReadStream('my-file.text')
stream.on('data', function(chunk){//处理文件部分内容})
stream.on('end', function(chunk){//文件读取完毕})
```

### 监视

node允许监视文件和目录是否发生变化，

```
var file = fs.readdirSync('')
file.forEach(funciton(item){
  if(/\.css/.test(file)) {
    fs.watchFile(process.cwd() + '/' + file, function(){
    })
  }
})

fs.watch()
```