# Python

## Conda

[官网](https://www.anaconda.com/products/individual)

```
# 查看虚拟环境
conda env list

# 创建环境
conda create -n your_env_name python=X.X
conda create -n python36 python=3.6.8

# 安装必要的包
conda create -n python36 numpy matplotlib python=3.6.8
# 或者
conda install -n your_env_name [package]


# 切换到虚拟环境
conda activate python36

# 关闭虚拟环境
conda deactivate python36

# 移除虚拟环境
conda remove -n your_env_name(虚拟环境名称) --all

# 移除虚拟环境中的某个包
conda remove --name $your_env_name  $package_name

# pip 查看 [package] 当前版本
pip show numpy
pip install numpy==1.16.0
```

## 配置 pip 国内镜像

```
# 命令
pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple/

# 国内常用镜像如下：
阿里云 http://mirrors.aliyun.com/pypi/simple/
中国科技大学 https://pypi.mirrors.ustc.edu.cn/simple/
豆瓣(douban) http://pypi.douban.com/simple/
清华大学 https://pypi.tuna.tsinghua.edu.cn/simple/
中国科学技术大学 http://pypi.mirrors.ustc.edu.cn/simple/

# pip 升级
pip install --upgrade pip
```

windows 直接在 user 目录中创建一个 pip 目录，如：C:\Users\Administrator\pip，新建文件 pip.ini，内容如下

Linux 上的文件路径如：~/.pip/pip.conf

```
[global]
index-url = https://pypi.tuna.tsinghua.edu.cn/simple
```
