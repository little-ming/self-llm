# llm-QuicklyDeploy

&emsp;&emsp;什么是大模型？

>大模型（LLM）狭义上指基于深度学习算法进行训练的自然语言处理（NLP）模型，主要应用于自然语言理解和生成等领域，广义上还包括机器视觉（CV）大模型、多模态大模型和科学计算大模型等。

&emsp;&emsp;如今国内外已经涌现了太多的开源大模型，国内也有很多优秀的开源大模型如：InternLM（书生·蒲语），ChatGLM，Qwen（通义千问），Yi（零一万物）等等。当前普通学生和用户想要使用这些大模型，需要具备一定的技术能力，才能完成模型的部署和使用。本项目旨在简化大模型的部署和使用，让更多的人能够使用大模型，利用大模型更好的学习和工作。

&emsp;&emsp;本项目基于AutoDL快速部署开源大模型，更适合中国宝宝的部署教程。 **欢迎各位大模型届未来的大佬** 提交PR，一起完善本项目。

# 模型目录

- InternLM
  - [x] [InternLM-Chat-7B](InternLM/01-InternLM-Chat-7B.md)
  - [x] [Lagent+InternLM-Chat-7B-V1.1](InternLM/02-Lagent+InternLM-Chat-7B-V1.1.md)
  - [x] [浦语灵笔图文理解&创作](InternLM/03-浦语灵笔图文理解&创作.md)
- ChatGLM
  - [x] [ChatGLM3-6B chat](ChatGLM/01-ChatGLM3-6B-chat.md)
  - [x] [ChatGLM3-6B Code Interpreter](ChatGLM/02-ChatGLM3-6B-Code-Interpreter.md)
- Qwen
  - [ ] Qwen-7B-chat
- Yi (暂时只有base模型，没有chat模型)
- 欢迎提交新模型
# 通用环境配置

## pip、conda 换源

更多详细内容可移步至[MirrorZ Help](https://help.mirrors.cernet.edu.cn/)查看。

### pip 换源

临时使用镜像源安装，如下所示：`some-package` 为你需要安装的包名

```shell
pip install -i https://mirrors.cernet.edu.cn/pypi/web/simple some-package
```

设置pip默认镜像源，升级 pip 到最新的版本 (>=10.0.0) 后进行配置，如下所示：

```shell
python -m pip install --upgrade pip
pip config set global.index-url https://mirrors.cernet.edu.cn/pypi/web/simple
```

如果您的 pip 默认源的网络连接较差，临时使用镜像源升级 pip：

```shell
python -m pip install -i https://mirrors.cernet.edu.cn/pypi/web/simple --upgrade pip
```

### conda 换源

镜像站提供了 Anaconda 仓库与第三方源（conda-forge、msys2、pytorch 等，各系统都可以通过修改用户目录下的 .condarc 文件来使用镜像站。

不同系统下的.condarc目录如下：

- Linux: ${HOME}/.condarc
- macOS: ${HOME}/.condarc
- Windows: C:\Users\<YourUserName>\.condarc

注意：

- Windows 用户无法直接创建名为 .condarc 的文件，可先执行 conda config --set show_channel_urls yes 生成该文件之后再修改。

快速配置

```shell
cat <<'EOF' > ~/.condarc
channels:
  - defaults
show_channel_urls: true
default_channels:
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/r
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/msys2
custom_channels:
  conda-forge: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  pytorch: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
EOF
```

## AutoDL 开放端口

将 `autodl `的端口映射到本地的 [http://localhost:6006](http://localhost:6006/) 仅在此处展示一次，以下两个 Demo 都是同样的方法把 `autodl `中的 `6006 `端口映射到本机的 `http://localhost:6006`的方法都是相同的，方法如图所示。

![Alt text](images/image-4.png)

## 模型下载

### hugging face

使用`huggingface`官方提供的`huggingface-cli`命令行工具。安装依赖:

```shell
pip install -U huggingface_hub
```

然后新建python文件，填入以下代码，运行即可。

- resume-download：模型名称
- local-dir：本地存储路径。（linux环境下需要填写绝对路径）

```python
import os

# 下载模型
os.system('huggingface-cli download --resume-download internlm/internlm-chat-7b --local-dir your_path')
```

### hugging face 镜像下载

与使用hugginge face下载相同，只需要填入镜像地址即可。使用`huggingface`官方提供的`huggingface-cli`命令行工具。安装依赖:

```shell
pip install -U huggingface_hub
```

然后新建python文件，填入以下代码，运行即可。

- resume-download：断点续下
- local-dir：本地存储路径。（linux环境下需要填写绝对路径）

```python
import os

# 设置环境变量
os.environ['HF_ENDPOINT'] = 'https://hf-mirror.com'

# 下载模型
os.system('huggingface-cli download --resume-download internlm/internlm-chat-7b --local-dir your_path')
```

更多关于镜像使用可以移步至 [HF Mirror](https://hf-mirror.com/) 查看。

### modelscope

使用`modelscope`中的`snapshot_download`函数下载模型，第一个参数为模型名称，参数`cache_dir`为模型的下载路径。

注意：`cache_dir`最好为决定路径。

安装依赖：
  
```shell
pip install modelscope
pip install transformers
```

在当前目录下新建python文件，填入以下代码，运行即可。

```python
import torch
from modelscope import snapshot_download, AutoModel, AutoTokenizer
import os
model_dir = snapshot_download('Shanghai_AI_Laboratory/internlm-chat-7b', cache_dir='your path', revision='master')
```


### git-lfs

来到[git-lfs](https://git-lfs.com/)网站下载安装包，然后安装`git-lfs`。安装好之后在终端输入`git lfs install`，然后就可以使用`git-lfs`下载模型了。当然这种方法需要你有一点点 **Magic** 。


```shell
git clone https://huggingface.co/internlm/internlm-7b
```

# 致谢

<div align=center>
  <a href="https://datawhale.club/#/">Datawhale</a>、
  <a href="https://www.shlab.org.cn/">上海人工智能实验室</a>
</div>