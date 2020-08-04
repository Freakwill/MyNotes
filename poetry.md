## [用 Poetry 创建并发Python布包](https://johnfraney.ca/posts/2019/05/28/create-publish-python-package-poetry/)

本文将介绍我对[Flake8 Markdown](https://pypi.org/project/flake8-markdown/)的`pyproject.toml`和codebase所做的更改，以使其可以在 [PyPI](https://pypi.org/)（python包索引）上发布。

## 创建一个包

 [Poetry 安装说明](https://poetry.eustace.io/docs/#installation)。用Poetry 创建一个包

```
$ poetry new flake8-markdown
Created package flake8-markdown in flake8-markdown
```



```
flake8-markdown/
├── flake8_markdown/
│   └── __init__.py
├── tests/
│   ├── __init__.py
│   └── test_flake8_markdown.py
├── pyproject.toml
└── README.rst
```



## 自定义包

 `pyproject.toml`:

```
[tool.poetry]
name = "flake8-markdown"
version = "0.1.1"
description = "Lints Python code blocks in Markdown files using flake8"
authors = ["John Franey <johnfraney@gmail.com>"]
# New attributes
license = "MIT"
readme = "README.md"
homepage = "https://github.com/johnfraney/flake8-markdown"
repository = "https://github.com/johnfraney/flake8-markdown"
keywords = ["flake8", "markdown", "lint"]
classifiers = [
    "Environment :: Console",
    "Framework :: Flake8",
    "Operating System :: OS Independent",
    "Topic :: Software Development :: Documentation",
    "Topic :: Software Development :: Libraries :: Python Modules",
    "Topic :: Software Development :: Quality Assurance",
]
include = [
    "LICENSE",
]

[tool.poetry.dependencies]
# Updated Python version
python = "^3.6"
# New dependency
flake8 = "^3.7"

[tool.poetry.dev-dependencies]
pytest = "^3.0"

# New scripts
[tool.poetry.scripts]
flake8-markdown = 'flake8_markdown:main'

[build-system]
requires = ["poetry>=0.12"]
build-backend = "poetry.masonry.api"
```

> 请参阅 [Poetry's `pyproject.toml` 文档](https://poetry.eustace.io/docs/pyproject/)。

### version

Version 描述了一个包的当前版本。我尝试遵循[语义版本控制](https://semver.org/spec/v2.0.0.html)，所以当更新向后不兼容时很明显。

### description

这个简短的描述出现在 `pip search` 和 [PyPI搜索结果](https://pypi.org/search/?q=flake8-markdown) 中。

### license

许可证出现在包的 PyPI 页面的“Meta”部分。

### readme

Poetry创建了一个 `README.rst` 文件，但我更喜欢用 Markdown 编写文档。


### homepage and repository

这些出现在包的 PyPI 页面的 Project 链接部分：

![PyPI project links](https://johnfraney.ca/images/flake8-markdown-pypi-project-links.png)

如果你的包有一个单独的文档网站，那么还有一个 `documentation` 属性。

### keywords

这些出现在 PyPI 页面的 Meta 部分：

![PyPI keywords](https://johnfraney.ca/images/flake8-markdown-pypi-keywords.png)

### classifiers

“Trove分类器”：

![PyPI classifiers](https://johnfraney.ca/images/flake8-markdown-pypi-classifiers.png)

完整列表见 [PyPI 分类器页面](https://pypi.org/classifiers/)。

### [tool.poetry.dependencies] :heart:

当使用普通的 [pip](https://github.com/pypa/pip) 或 [Pipenv](https://github.com/pypa/pipenv)管理包时，通常在两个位置指定包：dev和部署依赖项的`requirements.txt`或`Pipfile`，以及运行时/安装依赖项的`setup.py`。

Poetry对所有依赖项使用`pyproject.toml`，这简化了依赖项管理。

这部分包括两个更改：

1. 将最低的 Python 版本从3.7更新到3.6，以获得更广泛的兼容性
2. 添加`flake8`作为依赖项

### [tool.poetry.scripts]

[Scripts](https://poetry.eustace.io/docs/pyproject/#scripts) 是“安装包时将被安装的脚本或可执行文件”。换句话说，开发人员可以在这里根据函数创建CLI命令。

脚本的形式为：

```
script_name = '{package_name}:{function_name}'
```

Flake8 markdown 的CLI命令：

```
flake8-markdown = 'flake8_markdown:main'
```

运行 `flake8-markdown` 将从 `flake8_markdown/__init__.py`调用 `main()` 函数。

> 
> 要使包成为一个[可运行模块](https://docs.python.org/3.7/library/runpy.html)，比如`python -m flake8-markdown`，它需要一个 `__main__.py`模块。 `__main__.py` 文件导入并运行与上述脚本相同的 `main()` 函数。

## 发布包

### TestPyPI

[TestPyPI](https://test.pypi.org/) 将包上传到TestPyPI 并从那里安装，可以帮助包维护人员避免发送损坏的包版本。

1. 构建包：

```
$ poetry build
```

2. 将测试PyPI添加为备用包存储库：

```
$ poetry config repositories.testpypi https://test.pypi.org/simple
```

3. 把包发布到TestPyPI：

```
$ poetry publish -r testpypi

Publishing flake8-markdown (0.1.1) to testpypi
Username:
Password:

 - Uploading flake8-markdown-0.1.1.tar.gz 100%
 - Uploading flake8_markdown-0.1.1-py3-none-any.whl 100%
```

4. 通过在 [testpypi.pypi.org](https://test.pypi.org/project/flake8-markdown/) 上查看包并在独立的虚拟环境中安装测试版本，验证包的外观和工作是否符合预期：

```
pip install --index-url https://test.pypi.org/simple/ flake8-markdown
```

### PyPI

发布到PyPI：

```
poetry publish
```

> 在将包上传到包索引之前，你需要[注册一个PyPI帐户](https://pypi.org/account/register/) 。此帐户与TestPyPI上的任何帐户都是独立的。
