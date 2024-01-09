---
alias: [C++, cpp, CPP, c++, C++环境, Cpp环境]
tags: [Linux/Cpp]
---

# 安装Clangd
1. 下载对应版本的 clangd<br>
    - [Linux](https://github.com/clangd/clangd/releases/download/17.0.3/clangd-linux-17.0.3.zip)
    - [Mac](https://github.com/clangd/clangd/releases/download/17.0.3/clangd-mac-17.0.3.zip)
    - [Windows](https://github.com/clangd/clangd/releases/download/17.0.3/clangd-windows-17.0.3.zip)
    - [阿里云盘(Linux/Mac/Windows)](https://www.alipan.com/drive/file/backup/659672377c087d3e4a8e43979fdd74515da5c23e)
    
1. 解压并配置环境
```bash
# 解压文件
unzip clangd_[].zip -d clangd
# 将clang移动到防止软件的文件夹
sudo mv clangd /path/clangd
# 将clangd软链接到环境搜索路径
sudo ln -s /path/clangd/bin/clangd /usr/local/bin/clangd
```

# clang-format
根据 [LLVM Debian/Ubuntu packages](https://apt.llvm.org/) 更换 apt 源,参考 [apt 换源](#apt%20换源%20Linux)，根据不同的系统安装不同版本的 clang-format
```bash
sudo apt install clang-format-x
sudo ln -s /bin/clang-format-x /bin/clang-format
```
## Error
出现 `GPG error: https://apt.llvm.org/jammy llvm-toolchain-jammy InRelease: The following signatures couldn't be verified because the public key is not available: NO_PUBKEY 15CF4D18AF4F7421` 时：
```bash
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 15CF4D18AF4F7421[NO_PUBKEY后面跟的内容]
```
## .clang-format
```yaml
  Language: Cpp
  # clang-format 3.3
  AccessModifierOffset: -4
  IndentWidth: 4
  # clang-format 3.8
  AlignAfterOpenBracket: DontAlign
  # clang-format 11
  AlignConsecutiveBitFields: AcrossEmptyLinesAndComments
  # clang-format 9
  AlignConsecutiveMacros: 
    Enabled: true
    AcrossEmptyLines: true
    AcrossComments: false
  # clang-format 17
  AlignConsecutiveShortCaseStatements: 
    Enabled: true
    AcrossEmptyLines: true
    AcrossComments: true
    AlignCaseColons: false
  # clang-format 5
  AlignEscapedNewlines: Left
  # clang-format 3.5
  AlignOperands: AlignAfterOperator
  # clang-format 3.5
  AllowShortBlocksOnASingleLine: Empty
  # clang-format 3.6
  AllowShortCaseLabelsOnASingleLine: true
  # clang-format 11
  AllowShortEnumsOnASingleLine: false
  # clang-format 3.5
  AllowShortFunctionsOnASingleLine: Empty
  # clang-format 3.3
  AllowShortIfStatementsOnASingleLine: Never
  # clang-format 9
  AllowShortLambdasOnASingleLine: Empty
  # clang-format 3.7
  AllowShortLoopsOnASingleLine: false
  # clang-format 3.8
  AlwaysBreakAfterReturnType: None
  # clang-format 3.4
  AlwaysBreakBeforeMultilineStrings: false
  # clang-format 3.4
  AlwaysBreakTemplateDeclarations: Yes
  # clang-format 3.7
  BinPackArguments: true
  # clang-format 3.7
  BinPackParameters: true
  # clang-format 12
  BitFieldColonSpacing: Both
  # clang-format 3.8
  BreakBeforeBraces: Custom
  BraceWrapping:
    AfterEnum: false
    AfterStruct: false
    SplitEmptyFunction: false
    AfterCaseLabel: false
    AfterClass: false
    AfterControlStatement: Never
    AfterFunction: false
    AfterNamespace: false
    AfterUnion: false
    AfterExternBlock: false
    BeforeCatch: false
    BeforeElse: false
    BeforeLambdaBody: false
    BeforeWhile: false
  # clang-format 5
  BreakConstructorInitializers: BeforeComma
  # clang-format 3.9
  BreakStringLiterals: true
  # clang-format 3.7
  ColumnLimit: 90
  # clang-format 12
  EmptyLineBeforeAccessModifier: Always
  # clang-format 6
  IncludeBlocks: Regroup
  # clang-format 3.8
  IncludeCategories:
    - Regex:           '^<[^\.]+>'
      Priority:        1
      SortPriority:    2
      CaseSensitive:   true
    - Regex:           '^<.*\..*>'
      Priority:        2
    - Regex:           '<[[:alnum:].]+>'
      Priority:        3
    - Regex:           '^".*\..*"'
      Priority:        4
      SortPriority:    0
  # clang-format 16
  InsertNewlineAtEOF: true
  # clang-format 13
  LambdaBodyIndentation: Signature
  # clang-format 16
  LineEnding: LF
  # clang-format 3.7
  MaxEmptyLinesToKeep: 1
  # clang-format 3.7
  NamespaceIndentation: None
  # clang-format 14
  PackConstructorInitializers: BinPack
  # clang-format 3.7
  PointerAlignment: Left
  # clang-format 13
  ReferenceAlignment: Left
  # clang-format 3.7
  UseTab: Never
```
