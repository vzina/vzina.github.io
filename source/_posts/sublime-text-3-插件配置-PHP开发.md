---
title: sublime text 3 插件配置(PHP开发)
top: false
cover: false
toc: true
mathjax: true
date: 2020-07-06 23:42:41
password:
summary: 打造高效开发的sublime开发工具（PHP开发）。。。
tags:
  - sublime
  - php
categories:
  - 工具
---


## 一、安装插件控制台

* 单击 Preferences > Browse Packages...菜单
* 浏览一个文件夹，然后进入Installed Packages文件夹
* 下载软件包[Control.sublime-package](https://packagecontrol.io/Package%20Control.sublime-package)并将其复制到Installed Packages /目录中
* 重新启动Sublime文本

## 二、插件列表

1. `ConvertToUTF8`: 格式转换。
2. `SublimeLinter`: 语法高亮。
3. `Emmet`:    前端专用。
4. `SideBarEnhancements` : 扩展了侧边栏中菜单选项的数量，从而提升你的工作效率。[中文版](https://github.com/52fisher/SideBarEnhancements)
5. `SyncedSideBar`  自动在左边文件夹树中定位当前文件
6. `DocBlockr`：自动生成大块的注释，并且可以用tab在不同内容之间切换，很爽的。
7. `BracketHighlighter`：括弧高亮显示。
8. `AllAutocomplete`：Sublime自带的可以对当前文件中的变量和函数名进行自动提示，但是AllAutocomplete可以对打开的所有文件的变量名进行提示，增强版的代码自动提示符。
9. `AutoFileName`: 自动提示路径
10. [PHP Companion](https://github.com/erichard/SublimePHPCompanion.git)：php函数跳转工具
11. `Phpfmt` (php>=7.0)：自动格式化代码，详细配置如下：

```json
{
    "autocomplete": true,
    "enable_auto_align": true,
    "format_on_save": false,
    "indent_with_space": true,
    "passes":
    [
        "MergeElseIf"
    ],
    "psr2": true,
    "version": 4
}
```

12. `sublime-phpcs(windows)`：[php语法检测工具](https://github.com/benmatselby/sublime-phpcs.git)，
* 第一步：下载[php code sniffer插件安装包](https://github.com/benmatselby/sublime-phpcs)
解压安装包得到sublime-phpcs-master，把sublime-phpcs-master文件夹放到sublime安装目录下的Data/Packages/目录下；重启sublime, 打开Sublime Text 3->Preferences->Package Settings -> Php Code Sniffer 证明插件安装成功；
* 第二步：下载[php-cs-fixer.phar](http://cs.sensiolabs.org/get/php-cs-fixer.phar)
* 第三步：把php-cs-fixer.phar 放到你的 php.exe 安装目录；
* 第四步：进入php安装目录，下载并安装[pear](http://pear.php.net/go-pear.phar)

```shell
php go-pear.phar
```

* 第五步：安装PHP_CodeSniffer扩展,

```shell
#（默认安装方式即可）;
pear install PHP_CodeSniffer
# 检查是否安装成功(The installed coding standards are MySource, PEAR, PHPCS, PSR1, PSR2, Squiz and Zend)
phpcs -i
```

* 第六步：修改配置。

```
{
    // We want debugging on
    "show_debug": true,

    // Only execute the plugin for php files
    "extensions_to_execute": ["php"],

    // Do not execute for twig files
    "extensions_to_blacklist": ["twig.php"],

    // Execute the sniffer on file save
    "phpcs_execute_on_save": true,

    // Show the error list after save.
    "phpcs_show_errors_on_save": true,

    // Show the errors in the gutter
    "phpcs_show_gutter_marks": true,

    // Show outline for errors
    "phpcs_outline_for_errors": true,

    // Show the errors in the status bar
    "phpcs_show_errors_in_status": true,

    // Show the errors in the quick panel so you can then goto line
    "phpcs_show_quick_panel": true,

    // Path to php on windows installation
    // This is needed as we cannot run phars on windows, so we run it through php
    "phpcs_php_prefix_path": "D:\\php\\php-7.0.7\\php.exe",

    // We want the fixer to be run through the php application
    "phpcs_commands_to_php_prefix": ["Fixer"],


    // PHP_CodeSniffer settings
    // Yes, run the phpcs command
    "phpcs_sniffer_run": true,

    // And execute it on save
    "phpcs_command_on_save": true,

    // This is the path to the bat file when we installed PHP_CodeSniffer
    "phpcs_executable_path": "D:\\php\\php-7.0.7\\phpcs.bat",

    // I want to run the PSR2 standard, and ignore warnings
    "phpcs_additional_args": {
        "--standard": "PSR2",
        "-n": ""
    },


    // PHP-CS-Fixer settings
    // Don't want to auto fix issue with php-cs-fixer
    "php_cs_fixer_on_save": false,

    // Show the quick panel
    "php_cs_fixer_show_quick_panel": true,

    // The fixer phar file is stored here:
    "php_cs_fixer_executable_path": "D:\\php\\php-7.0.7\\php-cs-fixer.phar",

    // Additional arguments, run all levels of fixing
    "php_cs_fixer_additional_args": {
        "--level": "all"
    },


    // PHP Linter settings
    // Yes, lets lint the files
    "phpcs_linter_run": true,

    // And execute that on each file when saved (php only as per extensions_to_execute)
    "phpcs_linter_command_on_save": true,

    // Path to php
    "phpcs_php_path": "D:\\php\\php-7.0.7\\php.exe",

    // This is the regex format of the errors
    "phpcs_linter_regex": "(?P<message>.*) on line (?P<line>\\d+)",


    // PHP Mess Detector settings
    // Not turning on the mess detector here
    "phpmd_run": false,
    "phpmd_command_on_save": false,
    "phpmd_executable_path": "",
    "phpmd_additional_args": {}
}
```

1、mac方式：

```shell
# 安装phpcs
brew install php-code-sniffer
# 安装php-cs-fixer
brew install php-cs-fixer
```

2、mac配置：
```json
{
    // We want debugging on
    "show_debug": true,

    // Only execute the plugin for php files
    "extensions_to_execute": ["php"],

    // Do not execute for twig files
    "extensions_to_blacklist": ["twig.php"],

    // Execute the sniffer on file save
    "phpcs_execute_on_save": true,

    // Show the error list after save.
    "phpcs_show_errors_on_save": true,

    // Show the errors in the gutter
    "phpcs_show_gutter_marks": true,

    // Show outline for errors
    "phpcs_outline_for_errors": true,

    // Show the errors in the status bar
    "phpcs_show_errors_in_status": true,

    // Show the errors in the quick panel so you can then goto line
    "phpcs_show_quick_panel": true,

    // Path to php on windows installation
    // This is needed as we cannot run phars on windows, so we run it through php
    "phpcs_php_prefix_path": "/usr/local/opt/php71/bin/php",

    // We want the fixer to be run through the php application
    "phpcs_commands_to_php_prefix": ["Fixer"],


    // PHP_CodeSniffer settings
    // Yes, run the phpcs command
    "phpcs_sniffer_run": true,

    // And execute it on save
    "phpcs_command_on_save": true,

    // This is the path to the bat file when we installed PHP_CodeSniffer
    "phpcs_executable_path": "/usr/local/opt/php-code-sniffer/bin/phpcs",

    // I want to run the PSR2 standard, and ignore warnings
    "phpcs_additional_args": {
        "--standard": "PSR2",
        "-n": ""
    },


    // PHP-CS-Fixer settings
    // Don't want to auto fix issue with php-cs-fixer
    "php_cs_fixer_on_save": false,

    // Show the quick panel
    "php_cs_fixer_show_quick_panel": true,

    // The fixer phar file is stored here:
    "php_cs_fixer_executable_path": "/usr/local/opt/php-cs-fixer/bin/php-cs-fixer",

    // Additional arguments, run all levels of fixing
    "php_cs_fixer_additional_args": {
        "--level": "all"
    },


    // PHP Linter settings
    // Yes, lets lint the files
    "phpcs_linter_run": true,

    // And execute that on each file when saved (php only as per extensions_to_execute)
    "phpcs_linter_command_on_save": true,

    // Path to php
    "phpcs_php_path": "/usr/local/opt/php71/bin/php",

    // This is the regex format of the errors
    "phpcs_linter_regex": "(?P<message>.*) on line (?P<line>\\d+)",


    // PHP Mess Detector settings
    // Not turning on the mess detector here
    "phpmd_run": false,
    "phpmd_command_on_save": false,
    "phpmd_executable_path": "",
    "phpmd_additional_args": {}
}
```

## 三、配置快捷键：
```json
[
  {
    "keys": [
      "f6"
    ],
    "command": "expand_fqcn"
  },
  {
    "keys": [
      "shift+f6"
    ],
    "command": "expand_fqcn",
    "args": {
      "leading_separator": true
    }
  },
  {
    "keys": [
      "f5"
    ],
    "command": "find_use"
  },
  {
    "keys": [
      "f4"
    ],
    "command": "import_namespace"
  },
  {
    "keys": [
      "f3"
    ],
    "command": "implement"
  },
  {
    "keys": [
      "shift+f12"
    ],
    "command": "goto_definition_scope"
  },
  {
    "keys": [
      "f7"
    ],
    "command": "insert_php_constructor_property"
  },
  {
    "keys": [
      "alt+b"
    ],
    "command": "jump_back"
  },
  {
    "keys": [
      "alt+shift+b"
    ],
    "command": "jump_forward"
  }
]
```
12. `a file icon` 漂亮文件图标


## 四、个人使用配置：
```json
{
    "always_prompt_for_file_reload": false,
    "always_show_minimap_viewport": false,
    "animation_enabled": true,
    "atomic_save": false,
    "auto_close_tags": true,
    "auto_complete": true,
    "auto_complete_commit_on_tab": false,
    "auto_complete_cycle": false,
    "auto_complete_delay": 50,
    "auto_complete_selector": "source - comment, meta.tag - punctuation.definition.tag.begin",
    "auto_complete_size_limit": 4194304,
    "auto_complete_triggers":
    [
        {
            "characters": "<",
            "selector": "text.html"
        }
    ],
    "auto_complete_with_fields": false,
    "auto_find_in_selection": false,
    "auto_indent": true,
    "auto_match_enabled": true,
    "binary_file_patterns":
    [
        "*.jpg",
        "*.jpeg",
        "*.png",
        "*.gif",
        "*.ttf",
        "*.tga",
        "*.dds",
        "*.ico",
        "*.eot",
        "*.pdf",
        "*.swf",
        "*.jar",
        "*.zip"
    ],
    "bold_folder_labels": false,
    "caret_extra_bottom": 0,
    "caret_extra_top": 0,
    "caret_extra_width": 0,
    "caret_style": "smooth",
    "close_windows_when_empty": false,
    "color_scheme": "Packages/User/SublimeLinter/Monokai (SL).tmTheme",
    "copy_with_empty_selection": true,
    "create_window_at_startup": false,
    "default_encoding": "UTF-8",
    "default_line_ending": "unix",
    "detect_indentation": true,
    "dictionary": "Packages/Language - English/en_US.dic",
    "drag_text": true,
    "draw_centered": false,
    "draw_indent_guides": true,
    "draw_minimap_border": false,
    "draw_white_space": "selection",
    "enable_hexadecimal_encoding": true,
    "enable_tab_scrolling": true,
    "enable_telemetry": "auto",
    "ensure_newline_at_eof_on_save": false,
    "fade_fold_buttons": true,
    "fallback_encoding": "Western (Windows 1252)",
    "file_exclude_patterns":
    [
        "*.pyc",
        "*.pyo",
        "*.exe",
        "*.dll",
        "*.obj",
        "*.o",
        "*.a",
        "*.lib",
        "*.so",
        "*.dylib",
        "*.ncb",
        "*.sdf",
        "*.suo",
        "*.pdb",
        "*.idb",
        ".DS_Store",
        "*.class",
        "*.psd",
        "*.db",
        "*.sublime-workspace"
    ],
    "find_selected_text": true,
    "fold_buttons": true,
    "folder_exclude_patterns":
    [
        ".svn",
        ".git",
        ".hg",
        "CVS"
    ],
    "font_face": "Courier New",
    "font_options":
    [
    ],
    "font_size": 14,
    "gpu_window_buffer": "auto",
    "gutter": true,
    "highlight_line": true,
    "highlight_modified_tabs": true,
    "hot_exit": true,
    "ignored_packages":
    [
        "Vintage"
    ],
    "indent_guide_options":
    [
        "draw_normal"
    ],
    "indent_subsequent_lines": true,
    "indent_to_bracket": false,
    "index_exclude_patterns":
    [
        "*.log"
    ],
    "index_files": true,
    "index_workers": 0,
    "line_numbers": true,
    "line_padding_bottom": 2,
    "line_padding_top": 2,
    "margin": 4,
    "match_brackets": true,
    "match_brackets_angle": false,
    "match_brackets_braces": true,
    "match_brackets_content": true,
    "match_brackets_square": true,
    "match_selection": true,
    "match_tags": true,
    "move_to_limit_on_up_down": false,
    "open_files_in_new_window": false,
    "overlay_scroll_bars": "system",
    "preview_on_click": true,
    "remember_full_screen": false,
    "rulers":
    [
    ],
    "save_on_focus_lost": false,
    "scroll_past_end": true,
    "scroll_speed": 1.0,
    "shift_tab_unindent": false,
    "show_encoding": true,
    "show_full_path": true,
    "show_line_endings": false,
    "show_panel_on_build": true,
    "show_tab_close_buttons": true,
    "smart_indent": true,
    "spell_check": false,
    "spelling_selector": "markup.raw, source string.quoted - punctuation - meta.preprocessor.c.include, source comment - source comment.block.preprocessor, -(source, constant, keyword, storage, support, variable, markup.underline.link, meta.tag)",
    "tab_completion": true,
    "tab_size": 4,
    "theme": "Default.sublime-theme",
    "translate_tabs_to_spaces": true,
    "tree_animation_enabled": true,
    "trim_automatic_white_space": true,
    "trim_trailing_white_space_on_save": true,
    "update_check": false,
    "use_simple_full_screen": false,
    "use_tab_stops": true,
    "word_separators": "./\\()\"'-:,.;<>~!@#$%^&*|+=[]{}`~?",
    "word_wrap": true,
    "wrap_width": 0
}
```

## 五、PHP开发插件：
- smarty     https://github.com/amitsnyderman/sublime-smarty.git
- 代码片段 https://github.com/gerardroche/sublime-php-snippets

## 六、goto跳转设置：
- Linux - create "Default (Linux).sublime-mousemap" in ~/.config/sublime-text-3/Packages/User
- Mac - create "Default (OSX).sublime-mousemap" in ~/Library/Application Support/Sublime Text 3/Packages/User
- Win - create "Default (Windows).sublime-mousemap" in %appdata%\Sublime Text 3\Packages\User

```json
[
    {
        "button": "button1",
        "count": 1,
        "modifiers": ["ctrl"],
        "press_command": "drag_select",
        "command": "goto_definition"
    }
]
```

## 结束
待续...

## 参考链接

- [Sublime Text Guide](https://nicesu.gitbooks.io/sublime-text-guide/content/index.html)

#### FAQ:
1. Sublime Text 3 安装包失败，提示 Host codeload.github.com returned an invalid certificate,修复方式如下:
```json
{
    "bootstrapped": true,
    "debug": true,
    "installed_packages":
    [
        "Package Control"
    ],
    "downloader_precedence":
    {
        "linux": [ "curl", "urllib",    "wget" ],
        "osx": [ "curl", "urllib" ],
        "windows": [ "wininet" ]
    },
}
```