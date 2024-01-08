# RIME 輸入法 lua 插件

## 簡介

本倉庫包含了部分爲宇浩輸入法編寫的 RIME 輸入平臺 lua 插件.

這部分插件具有跨輸入法通用性,且能有效實現目前 RIME 輸入平臺所缺失的常用功能.使用户可以通過菜單或快捷鍵開啟或關閉:

- [輸入候選預測](./lua/yuhao/yuhao_autocompletion_filter.lua).
- [生僻字過濾或后置](./lua/yuhao/yuhao_char_filter.lua).用户可以自定義常用字集.它可進一步包裝爲兩個獨立的 lua 文件,實現[常用字優先](./lua/yuhao/yuhao_char_first.lua)和[只出常用字](./lua/yuhao/yuhao_char_only.lua).
- [全碼只出單字](./lua/yuhao/yuhao_single_char_only_for_full_code.lua).當用户輸入四碼時,只出單字,不出詞語.但一二三碼不受此影響.適合單字簡詞黨.
- [將候選中的簡化字前置](./lua/yuhao/yuhao_sc_first.lua).包括《通用規範漢字表》中的8105字.
- [將候選中的臺標傳統字前置](./lua/yuhao/yuhao_tw_first.lua).包括《常用國字》中的4808字.
- [將候選中的陸標傳統字前置](./lua/yuhao/yuhao_sc_first.lua).包括《古籍通規》中的14250字.

## 使用

### 老版本 rime-lib

第一步,将 [lua/yuhao/](./lua/yuhao/) 中的相關lua文件拷貝到你電腦中的 Rime/lua/yuhao 文件夾下.

第二步,在 rime.lua 中添加代碼激活本腳本,例如:

```lua
yuhao_char_filter = require("yuhao_char_filter")
yuhao_char_first = yuhao_char_filter.yuhao_char_first
yuhao_char_only = yuhao_char_filter.yuhao_char_only
```

第三步,在 engine/filters 區塊中添加:

```yaml
- lua_filter@yuhao_char_first
- lua_filter@yuhao_char_only
```

第四步,在 schema 文件的 switches 區塊中添加狀態:

```yaml
-- - options: [yuhao_char_only, yuhao_char_first, utf8]
-- states: [常用字過濾, 常用字前置, 全字集]
-- reset: 1
```

### 新版本 rime-lib

新 Rime 引擎使用模塊化導入,故第二步可以省略,只需將第三步改爲:

```yaml
- lua_filter@*yuhao.yuhao_char_first
- lua_filter@*yuhao.yuhao_char_only
```