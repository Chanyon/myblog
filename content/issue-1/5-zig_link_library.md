### 使用头文件+c文件方式
```
//build.zig
exe.addCSourceFile(.{.path="hello.c"});
exe.addIncludeDir(.{.path = "include/"});
exe.linkLibC();

// use
const c = @cImport({
    @cInclude("hello.h");
});
c.hello();
```

### 使用头文件+库方式
```
//build.zig
exe.addIncludeDir(.{.path = "include/"});
exe.addLibraryPath("lib"); // lib/hello.lib
exe.linkSystemLibrary("hello"); // -l hello

// use 
const c = @cImport({
  @cInclude("hello.h");
});

c.hello();
```

### 使用C-ABI+库方式
```
// build.zig
const lib = b.addStaticLibrary(.{}); // 生成静态库
lib.addIncludeDir(.{});
lib.addCSourceFile(.{});
lib.linkLibC(); // or lib.linkLibCpp();

const exe = b.addExe(.{});
exe.addLibrary(lib); // 使用静态库

// main.zig
extern "c" hello() void; // bind
pub fn main() !void {
  hello();
}
```