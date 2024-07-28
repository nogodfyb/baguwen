### 1.`du`命令
`du`（disk usage）命令用于估算文件和目录的磁盘使用情况。
#### 基本用法
```
du [选项] [文件或目录]
```
#### 常用选项

- `-h`：以人类可读的格式显示（例如，K、M、G）。
- `-s`：显示总计（不显示子目录的详细信息）。
- `-a`：包括所有文件和目录。
- `-c`：显示总计。
- `--max-depth=N`：限制显示的目录层级深度。
#### 示例

- 查看当前目录下每个文件和子目录的磁盘使用情况：
```
du -h
```

- 查看指定目录的总磁盘使用情况：
```
du -sh /path/to/directory
```

- 查看指定目录及其子目录的磁盘使用情况，限制深度为1：
```
du -h --max-depth=1 /path/to/directory
```
### 2.`df`命令
`df`（disk free）命令用于查看文件系统的磁盘使用情况。
#### 基本用法
```
df [选项] [文件或目录]
```
#### 常用选项

- `-h`：以人类可读的格式显示（例如，K、M、G）。
- `-T`：显示文件系统类型。
- `-i`：显示inode使用情况。
#### 示例

- 查看所有挂载的文件系统的磁盘使用情况：
```
df -h
```

- 查看特定目录所在的文件系统的磁盘使用情况：
```
df -h /path/to/directory
```
### 3.`ncdu`工具
`ncdu`（NCurses Disk Usage）是一个基于ncurses的磁盘使用分析工具，提供了交互式界面。
#### 安装
在Debian/Ubuntu系统上：
```
sudo apt-get install ncdu
```
在CentOS/RHEL系统上：
```
sudo yum install ncdu
```
#### 使用
```
ncdu /path/to/directory
```
### 4.`ls`命令
`ls`命令也可以显示文件的大小，但不如`du`详细。
#### 示例

- 以人类可读的格式显示文件大小：
```
ls -lh
```

- 显示目录的总大小：
```
ls -lhS
```
### 5.`find`命令结合`du`
可以使用`find`命令查找特定条件的文件，然后结合`du`命令查看它们的磁盘使用情况。
#### 示例

- 查找大于100MB的文件并显示其磁盘使用情况：
```
find /path/to/directory -type f -size +100M -execdu -h {} +
```
