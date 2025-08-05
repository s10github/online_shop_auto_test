## 一.项目介绍

该项目是一个在线购物的商城网站，包括用户注册，登录，下单，上架/下架商品，下单支付等相关功能；

## 二.项目结构说明

```tex
project-root/                          # 项目根目录
├─ base/                               # 基础类封装（核心功能）
│  ├─ api_request.py                   # 接口请求基类（封装requests）
│  ├─ test_case_tools.py               # 测试用例工具类（如数据处理、断言）
│  └─ driver.py                        # 浏览器驱动封装（可选，UI自动化用）
├─ common/                             # 公共方法封装（可复用工具）
│  ├─ log_utils.py                     # 日志处理工具
│  ├─ excel_parser.py                  # Excel数据解析工具
│  ├─ yaml_parser.py                   # YAML数据解析工具
│  └─ db_operation.py                  # 数据库操作工具（如MySQL）
├─ conf/                               # 全局配置目录
│  ├─ config.ini                       # 环境配置文件（API地址、账号等）
│  ├─ allure_config.yml                # Allure报告自定义配置
│  └─ env.py                           # 环境变量管理文件
├─ data/                               # 测试数据目录
│  ├─ api_data/                        # 接口测试数据
│  │  ├─ login_data.yaml                # 登录接口测试用例数据
│  │  └─ order_data.xlsx                # 订单接口Excel数据
│  └─ ui_data/                          # UI自动化测试数据（可选）
│     └─ page_elements.yaml             # 页面元素定位数据
├─ logs/                               # 测试日志目录（自动生成）
│  └─ test_20231001.log                 # 按日期命名的日志文件
├─ report/                             # 测试报告目录
│  ├─ allure_html/                      # Allure交互式报告（自动生成）
│  ├─ tm_report/                        # TMReport表格报告（自动生成）
│  └─ report_config.py                  # 报告生成配置文件
├─ testcase/                           # 测试用例目录
│  ├─ api_test/                         # 接口测试用例
│  │  ├─ test_login.py                  # 登录接口测试类
│  │  └─ test_order.py                  # 订单接口测试类
│  └─ ui_test/                          # UI自动化测试用例（可选）
│     └─ test_homepage.py               # 首页UI测试类
├─ venv/                               # 虚拟环境目录（自动生成）
├─ conftest.py                         # pytest全局钩子文件（固定名称）
├─ environment.xml                     # Allure报告环境信息文件
├─ extract.yaml                        # 接口依赖参数存储文件
├─ pytest.ini                          # pytest配置文件（固定名称）
├─ requirements.txt                    # 第三方库依赖清单
└─ run.py                              # 主程序入口（执行测试和生成报告）
```

## 三、程序入口 run.py

```python
import shutil
import pytest
import os
import webbrowser
from conf.setting import REPORT_TYPE
 
if __name__ == '__main__':
 
    if REPORT_TYPE == 'allure':
        pytest.main(
            ['-s', '-v', '--alluredir=./report/temp', './testcase', '--clean-alluredir',
             '--junitxml=./report/results.xml'])
 
        shutil.copy('./environment.xml', './report/temp')
        os.system(f'allure serve ./report/temp')
 
    elif REPORT_TYPE == 'tm':
        pytest.main(['-vs', '--pytest-tmreport-name=testReport.html', '--pytest-tmreport-path=./report/tmreport'])
        webbrowser.open_new_tab(os.getcwd() + '/report/tmreport/testReport.html')
```
