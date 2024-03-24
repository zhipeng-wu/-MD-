## 初始化项目

```javascript
1. git新建仓库 项目名称

2. git clone git@github.com:zhipeng-wu/next-my-blog.git

3. 配置eslint stylelint Prottier
  3.1  vscode 安装eslint stylelint Prottier 插件
  3.2 yarn add eslint -D  
     .eslintrc.json 修改
     
     {
      "extends": ["next/core-web-vitals","eslint:recommended"]
     }
 3.3  yarn add stylelint stylelint-config-standard-scss -D
   	新建 .stylelintrc.json	

    {
      "extends": "stylelint-config-standard-scss"
    }
 3.4 vscode  ctrl+shift+P   搜索 codeAr   找到以下配置
 	 "editor.codeActionsOnSave": {    //保存代码的时候自动格式化css样式
            "source.fixAll.stylelint":  true,
       },
 3.5 vscode 安装 prettier 插件   
   新建 .prettierrc   
   {
  "arrowParens": "always",
  "bracketSpacing": true,
  "endOfLine": "lf",
  "htmlWhitespaceSensitivity": "css",
  "insertPragma": false,
  "jsxBracketSameLine":false,
  "jsxSingleQuote": false,
  "printWidth": 80,
  "proseWrap": "preserve",
  "quoteProps": "as-needed",
  "requirePragma": false,
  "semi": true,
  "singleQuote": true,
  "tabWidth": 2,
  "trailingComma": "es5",
  "useTabs": false,
  "vueIndentScriptAndStyle": false,
  "parser":"babel"
}

3.6 vscode settings.json
{
  "editor.fontSize": 18,
  "[vue]": {
    "editor.defaultFormatter": "Vue.volar"
  },
  "workbench.editor.enablePreview": false,
  "[html]": {
    "editor.defaultFormatter": "vscode.html-language-features"
  },
  "editor.tabSize": 2,
  "editor.formatOnSave": true,  //保存自动加分号和双引号之类的
    "editor.formatOnType": true,
    "editor.codeActionsOnSave": {
        "source.fixAll.eslint": true
    },
    "[css]": {
        "editor.defaultFormatter": "esbenp.prettier-vscode"
    },
    "[scss]": {
        "editor.defaultFormatter": "esbenp.prettier-vscode"
    }
}
```

## pages 路由

nextjs中 有页面即 路由的概念   ，会根据pages目录下的结构自动生成路由

![image-20231213203731970](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231213203731970.png)