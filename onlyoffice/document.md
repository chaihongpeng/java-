# config

## document

与文档有关的所有参数, 包含标题网址文件类型等

### fileType

定义查看或编辑源文件的类型

### key

服务器会缓存文档, 默认如果文档的key不变就优先从缓存中获取文档

### title

文件名

### url

获取源文件路径

## editorConfig

部分允许更改与编辑界面有关的参数

## documentType

定义要打开的文档类型

## events

### onAppReady

当应用程序加载到浏览器时调用

### onDocumentReady

当文档加载到文档编辑器时调用的函数

### onDocumentStateChange

用户修改文档时调用,正在编辑或已发送到编辑服务器时

## height

文档在浏览器中的高度,默认100%

## width

文档在浏览器中的宽度,默认100%

## token

以令牌的形式定义添加到文档服务器的加密密钥

## type

定义用于访问文档的平台类型