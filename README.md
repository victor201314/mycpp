# mycpp — 多章节 C++ 项目模板

一个目录 = 一个 git 仓库 = 一本 C++ 教材/练习集。每章(`chapt01`、`chapt02`…)
是一个**完全独立的 CMake 项目**,互不依赖。

## 目录结构

```
mycpp/
├── .github/workflows/build.yml   # CI:push / PR 时自动构建所有章节
├── .gitignore                    # 忽略各章 build/、target/
├── README.md
└── chapt-template/               # 章节模板:复制它新建章节
    ├── CMakeLists.txt
    ├── .vscode/                  # clangd + CodeLLDB 开箱配置
    ├── include/print.h
    └── src/main.cpp, print.cpp
```

## 新建章节三步走

```bash
# 1. 复制模板(零修改:target 名自动取目录名)
cp -R chapt-template chapt01

# 2. 打开章节目录(注意:打开 chapt01,不是 mycpp 顶层)
code chapt01

# 3. F5 调试(首次自动构建)
```

不用 VSCode 时,命令行等价操作:

```bash
cmake -S chapt01 -B chapt01/build -DCMAKE_BUILD_TYPE=Debug
cmake --build chapt01/build
./chapt01/target/chapt01

git add chapt01 && git commit -m "新增 chapt01"
```

## CI

push / PR 到 GitHub 时,自动构建所有含 `CMakeLists.txt` 的子目录
(含 chapt-template 自检)。任一章节构建失败则标红。

## 注意事项

- **打开的是章节目录**(`code chapt01`),顶层 mycpp 只是收纳目录 + git 仓库
- **目录名就是 target 名**:推荐字母开头(如 `chapt01`、`chapt-01`),
  不要含空格;数字开头在本机 CMake 4.4.3 实测可用,但旧版 CMake 行为
  未经验证,建议避免
- F5 前需已安装 CMake Tools 扩展,并至少完成一次 configure
  (打开章节目录后会自动执行);`launch.json` 依赖
  `${command:cmake.launchTargetPath}`,未 configure 时调试会报「找不到程序」
- 不要安装 ms-vscode.cpptools(其「Run C/C++ File」按钮在 macOS 上会因
  `lldb --interpreter=mi` 报错;智能提示已由 clangd 完全接管)
- 单项目场景请使用 `cpp-template` 模板

## 与原模板的关系

| 场景 | 用哪个模板 |
|------|-----------|
| 单个独立项目 | `cpp-template`(复制 + 全局改名) |
| 多章节教材/练习仓库 | `mycpp`(本模板,复制章节目录零改名) |
