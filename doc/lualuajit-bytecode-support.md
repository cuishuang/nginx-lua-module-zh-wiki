Lua/LuaJIT 字节码支持
===========================

从`v0.5.0rc32` release 开始，所有 `*_by_lua_file` 的配置小节例如 [content_by_lua_file](#content_by_lua_file) 都支持直接加载 Lua 5.1 和 LuaJIT 2.0/2.1 的二进制字节码文件。

请注意，由 LuaJIT 2.0/2.1 生成的二进制格式与标准 Lua 5.1 解析器是不兼容的。所以如果使用在ngx_lua下使用LuaJIT 2.0/2.1，那么LuaJIT兼容的二进制文件必需是下面这样生成的：

```bash

 /path/to/luajit/bin/luajit -b /path/to/input_file.lua /path/to/output_file.luac
```

`-bg` 选项是在 LuaJIT 字节码文件中包含调试信息。

```bash

 /path/to/luajit/bin/luajit -bg /path/to/input_file.lua /path/to/output_file.luac
```

对于`-b` 选项，请参考官方 LuaJIT 文档获取更多细节：

<http://luajit.org/running.html#opt_b>

同样的，由 LuaJIT 2.1 生成的字节码文件对于 LuaJIT 2.0 也是 *不* 兼容的，反之亦然。第一次对于 LuaJIT 2.1 版本的字节支持，是在 v0.9.3 上完成的。

近似的，如果使用标准Lua 5.1解析器，Lua 的兼容字节码文件是必须使用下面的命令行生成：

```bash

 luac -o /path/to/output_file.luac /path/to/input_file.lua
```

与LuaJIT不太一样，调试信息是默认被包含到字节码文件中的。这里可以使用 `-s` 选项剥离掉去掉调试信息：

```bash

 luac -s -o /path/to/output_file.luac /path/to/input_file.lua
```

相似的，加载标准Lua 5.1字节码文件到使用LuaJIT 2.0/2.1的ngx_lua实例中（反之亦然），将会在error日志中收到下面一条类似的错误：

    [error] 13909#0: *1 failed to load Lua inlined code: bad byte-code header in /path/to/test_file.luac

使用Lua`require` 和 `dofile`这类原语加载字节码文件，应该总能按照预期工作。

[Back to TOC](#table-of-contents)

> English source:

Lua/LuaJIT bytecode support
===========================

As from the `v0.5.0rc32` release, all `*_by_lua_file` configure directives (such as [content_by_lua_file](#content_by_lua_file)) support loading Lua 5.1 and LuaJIT 2.0/2.1 raw bytecode files directly.

Please note that the bytecode format used by LuaJIT 2.0/2.1 is not compatible with that used by the standard Lua 5.1 interpreter. So if using LuaJIT 2.0/2.1 with ngx_lua, LuaJIT compatible bytecode files must be generated as shown:

```bash

 /path/to/luajit/bin/luajit -b /path/to/input_file.lua /path/to/output_file.luac
```

The `-bg` option can be used to include debug information in the LuaJIT bytecode file:

```bash

 /path/to/luajit/bin/luajit -bg /path/to/input_file.lua /path/to/output_file.luac
```

Please refer to the official LuaJIT documentation on the `-b` option for more details:

<http://luajit.org/running.html#opt_b>

Also, the bytecode files generated by LuaJIT 2.1 is *not* compatible with LuaJIT 2.0, and vice versa. The support for LuaJIT 2.1 bytecode was first added in ngx_lua v0.9.3.

Similarly, if using the standard Lua 5.1 interpreter with ngx_lua, Lua compatible bytecode files must be generated using the `luac` commandline utility as shown:

```bash

 luac -o /path/to/output_file.luac /path/to/input_file.lua
```

Unlike as with LuaJIT, debug information is included in standard Lua 5.1 bytecode files by default. This can be striped out by specifying the `-s` option as shown:

```bash

 luac -s -o /path/to/output_file.luac /path/to/input_file.lua
```

Attempts to load standard Lua 5.1 bytecode files into ngx_lua instances linked to LuaJIT 2.0/2.1 or vice versa, will result in an error message, such as that below, being logged into the Nginx `error.log` file:


    [error] 13909#0: *1 failed to load Lua inlined code: bad byte-code header in /path/to/test_file.luac


Loading bytecode files via the Lua primitives like `require` and `dofile` should always work as expected.

[Back to TOC](#table-of-contents)