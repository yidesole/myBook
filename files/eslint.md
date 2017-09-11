# eslint笔记

#### eslint 是javaScript的代码规范工具

#### 安装eslint

###### 全局安装

```
    npm i eslint -g
```

###### 项目安装

```
    npm i eslint -dev
```

###### 初始化配置

```
    eslint --init
```

###### 检查代码

```
    eslint demo.js
```

###### 自动修复

```
    eslint demo.js --fix
```


###### 基本配置文件.eslintrc.js（规则配置文件）

```
    module.exports = {
        "env": {
            "browser": true,
            "commonjs": true,
            "es6": true,
            "node": true
        },
        "extends": "eslint:recommended",
        "parserOptions": {
            "ecmaFeatures": {   //使用jsx语法
                "jsx": true
            },
            "sourceType": "module"
        },
        "rules": {
            "indent": [ //缩进方式空格2个
                "error",
                2
            ],
            "linebreak-style": [
                "error",
                "unix"
            ],
    	"no-console":[  //可以使用console语句(默认不可以使用)	
    	    "off"
    	],
            "quotes": [ //设置双引号
                "error",
                "double"
            ],
            "semi": [
                "error",
                "never"
            ]
        }
    };
```



