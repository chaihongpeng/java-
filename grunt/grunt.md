# 目录结构

- build
- src
  - js
  - css
- index.html
- Gruntfile.js
- package.json

# 安装

```sh
npm install -g grunt-cli
npm install grunt --save-dev
```

# Gruntfile.js

```js
module.exports = function(grunt) {

  // 初始化配置
  grunt.initConfig({
  });

  // grunt任务执行时,加载对应的任务插件
  grunt.loadNpmTasks('grunt-contrib-uglify');

  // 注册grunt的默认任务,数组中标识执行默认任务时的依赖任务
  grunt.registerTask('default', ['uglify']);

};
```

# grunt常用插件

- grunt-contrib-clean: 清除文件(打包处理生成的)
- grunt-contrib-concat: 合并多个文件到一个文件中
- grunt-contrib-uglify: 压缩js文件
- grunt-contrib-jshint: jshint语法错误检查
- grunt-contrib-cssmin: 压缩/合并css文件
- grunt-contrib-htmlmin: 压缩html文件
- grunt-contrib-imagemin: 压缩图片文件(无损)
- grunt-contrib-copy: 复制文件
- grunt-contrib-watch: 实时监控文件变化, 调用相应的任务重新执行

## 插件使用

```sh
# 下载合并插件
npm install grunt-contrib-concat --save-dev
```

```js
module.exports = function (grunt) {
  grunt.initConfig({
    // 合并js配置
    concat: {// 任务名称
      options: {
        separator: ";"// 合并js间的分隔符
      },
      dist: {
        src: ["src/js/*.js"],// 合并文件的目录
        dest: "build/js/build.js"// 输出目录文件名
      },
    },
    pkg: grunt.file.readJSON('package.json'),
    uglify: {
      options: {
        //编译后的文件头注释信息
        banner: '/*! <%= pkg.name %> - v<%= pkg.version %> - ' +
          '<%= grunt.template.today("yyyy-mm-dd") %> */'
      },
      my_target: {
        files: {
          'build/build.min.js': ['build/js/build.js']
        }
      }
    }
  });

  // grunt任务执行时,加载对应的任务插件
  grunt.loadNpmTasks('grunt-contrib-concat');
  // 注册压缩文件插件
  grunt.loadNpmTasks('grunt-contrib-uglify');

  // 注册grunt的默认任务,数组中标识执行默认任务时的依赖任务
  // grunt执行任务是同步的
  grunt.registerTask('default', ['uglify']);

};
```

```sh
# 执行concat任务
grunt concat
# 默认执行default里的任务
grunt
```

