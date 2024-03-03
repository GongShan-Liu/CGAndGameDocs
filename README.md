# CGAndGameDocs

## 本文档是影视和游戏的相关技术的文档库

- 作者: yao
- 网址: [文档库](https://gongshan-liu.github.io/CGAndGameDocs/index.html)
- 问题留言: [Issues](https://github.com/GongShan-Liu/CGAndGameDocs/issues)

## 说明

本文档是作者本人从事影视和游戏行业的工作学习经验总结，涉及文档如存在描述错误或者误导，麻烦在留言区提出问题，本人看到相关问题会及时回复。

## 关于本文档版权

本文档的知识产权归作者所有，未经本人同意不可转载到其他网页或博客，可作为学习或交流下载分享; 本文档中声明了经他人同意转载或引用部分，如要转载等其他需经原作者同意部分, 请联系原作者

## 文档构建架构

本文档是使用 [jupyter book](https://jupyterbook.org/en/stable/intro.html) 架构来构建的 详细教程可以 [登录该网页](https://jupyterbook.org/en/stable/start/overview.html)

```python
'''
    windows cmd执行一下命令
        # 1. 安装jupyter-book
        pip install -U jupyter-book

        # 2. 任意路径下创建jupyter-book项目
        jupyter-book create mynewbook/

        # 3. 构建项目
        jupyter-book build mybookname/

        # 4. 发布项目 在github创建jupyter-book项目仓库 并 克隆到本地
        #     把已经创建好的本地jupyter-book文件夹的内容复制到 
        #     本地git克隆的文件夹下 并上传项目到github

        # 5. 安装 ghp-import
        pip install ghp-import

        # 6. 发布到GitHub Pages 成功后可打开 https://<user>.github.io/<myonlinebook>/ 
        ghp-import -n -p -f _build/html

'''
```