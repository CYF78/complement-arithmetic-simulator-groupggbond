# complement-arithmetic-simulator
补码加减法运算模拟器｜计算机与专业基础研讨课程大作业

## 📖 项目概述
本项目是二进制补码加减法运算模拟器，面向计算机基础课程学习，用于演示定点二进制补码的加法、减法运算完整流程。
在计算机中，减法通过「加上减数的补码实现，硬件不需要单独设计减法器；同时项目实现**双符号位溢出判断**、进位输出、真值、原码、补码对照展示，把底层运算过程可视化，方便理解补码运算原理、溢出产生条件。

支持自定义字长（4位、8位、16位等定点二进制），输入十进制或者二进制，自动完成转换、运算、溢出判定，输出每一步中间过程，而不是只给最终结果。

 适用场景：计基课程实验、补码原理学习、课堂演示。

## ✨ 核心功能
1. **字长自定义**：支持配置运算位数（4bit / 8bit / 16bit 定点补码）
2. **两种输入模式**：支持输入十进制整数，或者直接输入二进制串
3. **\([Y]_{\([-Y]_{补}\)}\)码加法**：$[X]_{\([Y]_{\([-Y]_{补}\)}\)}+[Y]_{\([Y]_{\([-Y]_{补}\)}\)}=[X+Y]_{\([Y]_{\([-Y]_{补}\)}\)}$
4. **\([Y]_{\([-Y]_{补}\)}\)码减法**：$[X]_{\([Y]_{\([-Y]_{补}\)}\)}-[Y]_{\([Y]_{\([-Y]_{补}\)}\)}=[X]_{\([Y]_{\([-Y]_{补}\)}\)}+[-Y]_{\([Y]_{\([-Y]_{补}\)}\)}$，自动求减数的负数\([Y]_{\([-Y]_{补}\)}\)码
5. **双符号位溢出判断**（推荐，课程标准方法）
   - 符号位 `00`：正数，无溢出
   - 符号位 `11`：负数，无溢出
   - 符号位 `01`：正溢出（结果超出正数表示范围）
   - 符号位 `10`：负溢出（结果超出负数表示范围）
6. **完整过程打印**
   - 输入真值
   - 对应原码、单符号\([Y]_{\([-Y]_{补}\)}\)码、双符号\([Y]_{\([-Y]_{补}\)}\)码
   - 按位相加过程、最高进位
   - 运算结果\([Y]_{\([-Y]_{补}\)}\)码、还原后的真值
7. **范围校验**：提前提示输入数字是否超出当前字长\([Y]_{\([-Y]_{补}\)}\)码可表示范围
8. 可选图形界面（Tkinter）：输入框、按钮、结果文本框，直观交互；也保留纯命令行版本用于测试

## 🧮 原理说明
> 定点\([Y]_{\([-Y]_{补}\)}\)码运算规则：
1. \([Y]_{\([-Y]_{补}\)}\)码加法：两个数的\([Y]_{\([-Y]_{补}\)}\)码直接相加，结果为和的\([Y]_{\([-Y]_{补}\)}\)码
2. \([Y]_{\([-Y]_{补}\)}\)码减法：$X-Y = X+(-Y)$，先对 $[Y]_{\([Y]_{\([-Y]_{补}\)}\)}$ 求 $[-Y]_{\([Y]_{\([-Y]_{补}\)}\)}$（所有位取反+1），再做加法
3. 溢出≠最高位进位：进位为1不代表溢出；溢出是**结果超出该字长\([Y]_{\([-Y]_{补}\)}\)码的表示范围**
4. 双符号位规则：
   - 正数\([Y]_{\([-Y]_{补}\)}\)码双符号：`00`
   - 负数\([Y]_{\([-Y]_{补}\)}\)码双符号：`11`
   - 相加后两个符号位不一致 → 溢出

以8位单符号\([Y]_{\([-Y]_{补}\)}\)码举例：
表示范围：$-128 \sim +127$

##  项目目录结构
```
complement-arithmetic-simulator/
├── README.md                                   # 项目说明文档（本文件）
├── Project_Requirements_and_Design.md          # 需求分析+概要设计文档（作业要求）
├── AI_Prompts_shturl                           # AI提示词记录（作业要求）
├── main.py                                     # 程序入口
├── complement_core.py                          # \([Y]_{\([-Y]_{补}\)}\)码核心算法模块
├── cli.py                                      # 命令行交互版本
├── gui.py                                      # Tkinter图形界面（可选）
├── tests/
│   └── test_complement.py                      # 单元测试用例：加法、减法、溢出案例
└── .gitignore                                  # Git忽略配置
```

##  技术栈
- 编程语言：Python 3.8+
- GUI：Python内置 Tkinter（无需额外安装）
- 无第三方复杂依赖，开箱即用

##  运行指南
### 1. 环境准备
确保本地 Python 版本 ≥3.8
```bash
python --version
```

### 2. 克隆仓库
```bash
git clone https://github.com/你的用户名/complement-arithmetic-simulator.git
cd complement-arithmetic-simulator
```

### 3. 运行命令行版本
```bash
python cli.py
```

### 4. 运行图形界面版本
```bash
python gui.py
```

###  使用示例（命令行）
```
===== \([Y]_{\([-Y]_{补}\)}\)码加减法模拟器 =====
请设置运算字长(4/8/16): 8
请选择运算：1-加法  2-减法
请输入选择：1
请输入X（十进制）：60
请输入Y（十进制）：50
----------运算过程----------
X真值：+60，原码：00111100，\([Y]_{\([-Y]_{补}\)}\)码：00111100，双符号\([Y]_{\([-Y]_{补}\)}\)码：00 00111100
Y真值：+50，原码：00110010，\([Y]_{\([-Y]_{补}\)}\)码：00110010，双符号\([Y]_{\([-Y]_{补}\)}\)码：00 00110010
双符号相加：
  00 00111100
+ 00 00110010
= 00 01101110
结果\([Y]_{\([-Y]_{补}\)}\)码：01101110，真值：110
符号位：00 → 无溢出
```

溢出示例：8位，X=90，Y=50，相加结果140，超出+127，触发正溢出
```
符号位：01 → 正溢出！
```

##  小组分工表
序号	                     负责内容
1	陈宇飞                         仓库搭建、分支管理、PR 审核、整体架构与进度协调
2	贺腾宾                         complement_core.py：补码转换、加减法、双符号位溢出判断核心算法
3 刘崇阳                        	gui.py：Tkinter 图形界面开发、交互布局、结果展示面板
4 宋沛翰                        	cli.py 命令行版本、tests/ 单元测试、边界用例与溢出案例验证
5  陈明泽                        	文档编写维护：README、需求设计文档、AI 提示词记录、提交记录整理序号	负责内容

##  Git协作规范
1. `main` 主分支：受分支保护规则限制，**禁止直接push**，所有修改必须走 PR(Pull Request)
2. 分支命名规范：
   - `feat/core-complement`：\([Y]_{\([-Y]_{补}\)}\)码核心算法开发
   - `feat/gui-window`：图形界面开发
   - `docs/requirement`：需求文档、README更新
   - `fix/overflow-bug`：修复溢出判断bug
   - `test/case`：新增测试用例
3. Commit 提交信息规范：`类型: 简短描述`
   - `feat: 实现双符号位溢出判断`
   - `docs: 完善README运算原理说明`
   - `fix: 修复负数求\([Y]_{\([-Y]_{补}\)}\)逻辑错误`
   - `test: 增加8位正溢出测试样例`
4. 开发流程：
   - 从main拉取新分支 → 本地开发 → commit → push远程 → 创建PR → 负责人Review → 确认无误合并进main
5. 作业要求：需要制造一次合并冲突并解决，保留提交记录用于评分

##  边界测试案例（可供测试）
| 字长 | X | Y | 运算 | 预期结果 |
|------|---|---|------|----------|
| 8bit | 60 | 50 | 加 | 110，无溢出 |
| 8bit | 90 | 50 | 加 | 正溢出 |
| 8bit | -100 | -50 | 加 | 负溢出 |
| 8bit | 30 | -20 | 减 | 50，无溢出 |
| 4bit | 5 | 2 | 加 | 正溢出（4位\([Y]_{\([-Y]_{补}\)}\)码范围 -8~7） |

##  开源协议
MIT License

Copyright (c) 2025-2026 本小组

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

---

