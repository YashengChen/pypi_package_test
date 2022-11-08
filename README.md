# 如何打包pypi Package
###### tags: `Python`, `Tutorial`
```
title: Facebook Crawler
developer: Rock Chen
updatetime: 2022-11-08
version: v0.0.1
```
## 1. Package Structure
```
packaging_tutorial/
├── LICENSE
├── pyproject.toml
├── README.md
├── src/
│   └── example_package_YOUR_USERNAME_HERE/
│       ├── __init__.py
│       └── example.py
└── tests/
```
## 2. Create Simple Pypi package
1. 建立套件範例 `example.py`
```
def add_one(number):
    return number + 1
```
2. tests/ 資料夾
> 通常會將測試檔案放置這裡，暫時留空
3. 建立 `pyproject.toml`
    - 設定安裝套件平台及需要套件 **`[build-system]`**:
        - 參數說明:
            - `requires`: 安裝套件所須使用的套件列表
            - `build-backend`: 安裝套件所使用的安裝平台來源
        
        - Hatchling:
            ```            
            [build-system]
            requires = ["hatchling"]
            build-backend = "hatchling.build"
            ```
        - setuptools
            ```
            [build-system]
            requires = ["setuptools>=61.0"]
            build-backend = "setuptools.build_meta"
            ```
        - Flit
            ```
            [build-system]
            requires = ["flit_core>=3.2"]
            build-backend = "flit_core.buildapi"
            ```
        - PDM
            ```
            [build-system]
            requires = ["pdm-pep517"]
            build-backend = "pdm.pep517.api"
            ```
    - 設定套件 屬性資料 **`[project]`**:
        ```
        [project]
        name = "example_package_YOUR_USERNAME_HERE"
        version = "0.0.1"
        authors = [
          { name="Example Author", email="author@example.com" },
        ]
        description = "A small example package"
        readme = "README.md"
        requires-python = ">=3.7"
        classifiers = [
            "Programming Language :: Python :: 3",
            "License :: OSI Approved :: MIT License",
            "Operating System :: OS Independent",
        ]
        ```
        - `name`: 套件名稱，只接受符號`.`, `_`& `-`
        - `version`: 套件版本，請遵循 [python version](https://packaging.python.org/en/latest/specifications/version-specifiers/#version-specifiers) 規則
        - `authours`: 套件作者資訊, 包含 名稱、電子信箱
        - `readme`: 說明檔案位置
        - `requires-python`: 套件所使用的python 版本需求
        - `classifiers`: 套件資訊
    - 設定套件網址 `[project.urls]`
        ```
        [project.urls]
        "Homepage" = "https://github.com/pypa/sampleproject"
        "Bug Tracker" = "https://github.com/pypa/sampleproject/issues"
        ```
        
4. 建立 `README.md`
```
# Example Package

This is a simple example package. You can use
[Github-flavored Markdown](https://guides.github.com/features/mastering-markdown/)
to write your content.
```
5. 建立 憑證 `LICENSE`
    建立憑證對每個套件來說很重要，憑證對於使用您套件的使用者，能夠規範他們能對您的套件做什麼，如何選擇憑證可透過 [ https://choosealicense.com/]( https://choosealicense.com/. ) 來協助您選擇適合您的憑證。
```
Copyright (c) 2018 The Python Packaging Authority

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```
6. 建立包含其他所需套件
    更新套件:
    - Unix/macOS: `python3 -m pip install --upgrade build`
    - Windows: `py -m pip install --upgrade build`
    
    以下這些套件透過指令自動產生:
    - Unix/macOS: `python3 -m build`
    - Windows: `py -m build`

    上述指令將產生以下檔案
        ```
        dist/
        ├── example_package_YOUR_USERNAME_HERE-0.0.1-py3-none-any.whl
        └── example_package_YOUR_USERNAME_HERE-0.0.1.tar.gz
        ```
7. 上傳 套件
    - 因為sample 將上傳到test/pypi 伺服器，故如無test/pypi 帳號，可透過 [https://test.pypi.org/account/register/](https://test.pypi.org/account/register/) 註冊
    - 更新套件:
        - Unix/macOS: `python3 -m pip install --upgrade twine`
        - Windows: `py -m pip install --upgrade twine`
    - 上傳套件:
        - Unix/macOS: `python3 -m twine upload --repository testpypi dist/*`
        - Windows: `py -m twine upload --repository testpypi dist/*`
    成功會輸出以下:
    ```
    Uploading distributions to https://test.pypi.org/legacy/
    Enter your username: __token__
    Uploading example_package_YOUR_USERNAME_HERE-0.0.1-py3-none-any.whl
    100% ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 8.2/8.2 kB • 00:01 • ?
    Uploading example_package_YOUR_USERNAME_HERE-0.0.1.tar.gz
    100% ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 6.8/6.8 kB • 00:00 • ?
    ```
    
8. 測試安裝上傳套件
- Unix/macOS: `python3 -m pip install --index-url https://test.pypi.org/simple/ --no-deps example-package-YOUR-USERNAME-HERE`
- Windows: `py -m pip install --index-url https://test.pypi.org/simple/ --no-deps example-package-YOUR-USERNAME-HERE`


9. 測試套件
```
>>> from example_package_YOUR_USERNAME_HERE import example
>>> example.add_one(2)
3
```