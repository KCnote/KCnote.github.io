---
layout: post
title: "filesystem (C++17)"
date: 2026-04-15 00:00:00 +0900
author: kang
categories: [CODE, CODE - Functions]
tags: [CODE, CODE - Functions]
pin: false
math: true
mermaid: true
---

# <b>filesystem (C++17)</b>

---

### <b>Prerequisites</b>

---

## <b>1. What is a filesystem?</b>

`<filesystem>` provides a **standard, portable way to work with files and directories**.

Before C++17:
- OS-specific APIs (POSIX / Windows)
- Boost.Filesystem

Now:
- Built into the standard library
- Cross-platform

```cpp
#include <filesystem>

namespace fs = std::filesystem;
```

| Component | Description |
|----------|-------------|
| `fs::path` | represents file path |
| `fs::directory_iterator` | iterate directory |
| `fs::exists` | check existence |
| `fs::create_directory` | create folder |
| `fs::remove` | delete file |
| `fs::file_size` | get file size |

Use it when:
- file/directory manipulation
- cross-platform file handling
- writing tools, scripts, utilities
- logging, file scanning

## <b>2. filesystem functions</b>

#### `fs::path`

Represents a file system path.

```cpp
fs::path p = "folder/file.txt";
```

```cpp
p.filename();        // "file.txt"
p.extension();       // ".txt"
p.stem();            // "file"
p.parent_path();     // "folder"
```

```cpp
fs::path p = "folder";
p /= "file.txt";   // folder/file.txt
```

`/=` is preferred over string concatenation

#### Checking Files

```cpp
if (fs::exists("file.txt"))
    std::cout << "File exists\n";
```

#### Type Check

```cpp
fs::is_regular_file(p);
fs::is_directory(p);
```

#### Directory Operations

##### Create Directory

```cpp
fs::create_directory("new_folder");
```

##### Create Nested

```cpp
fs::create_directories("a/b/c");
```

##### Remove

```cpp
fs::remove("file.txt");       // file
fs::remove_all("folder");     // recursive
```

##### Iterating Directory

```cpp
for (const auto& entry : fs::directory_iterator("folder"))
    std::cout << entry.path() << "\n";
```

```
folder/
 ├── a.txt
 ├── b.txt
 └── sub/
      └── c.txt

Result:
folder/a.txt
folder/b.txt
folder/sub
```

##### Recursive Iteration

```cpp
for (const auto& entry : fs::recursive_directory_iterator("folder"))
    std::cout << entry.path() << "\n";
```

```
folder/
 ├── a.txt
 ├── b.txt
 └── sub/
      └── c.txt

Result:
folder/a.txt
folder/b.txt
folder/sub
folder/sub/c.txt
```

#### File Size & Info

```cpp
auto size = fs::file_size("file.txt");
```

#### Last Modified Time

```cpp
auto time = fs::last_write_time("file.txt");
```

#### Copy / Move Files

```cpp
fs::copy("source.txt", "dest.txt");
```

#### Move / Rename

```cpp
fs::rename("old.txt", "new.txt");
```

#### Error Handling

```cpp
try
{
    fs::remove("file.txt");
}
catch (const fs::filesystem_error& e)
{
    std::cout << e.what();
}
```

#### Non-throwing Version

```cpp
std::error_code ec;
fs::remove("file.txt", ec);

if (ec)
    std::cout << ec.message();
```

## <b>3. Performance Considerations</b>

#### System Calls

Many operations call the OS:

- `exists`
- `file_size`
- `directory_iterator`

These are **relatively expensive**

- Avoid repeated `exists()` calls
- Cache results if possible
- Prefer batch operations
