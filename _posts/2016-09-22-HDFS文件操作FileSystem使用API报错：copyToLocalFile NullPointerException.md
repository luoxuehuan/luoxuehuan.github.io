### 出错：

```
Exception in thread "main" java.lang.NullPointerException
	at java.lang.ProcessBuilder.start(Unknown Source)
	at org.apache.hadoop.util.Shell.runCommand(Shell.java:482)
	at org.apache.hadoop.util.Shell.run(Shell.java:455)
	at org.apache.hadoop.util.Shell$ShellCommandExecutor.execute(Shell.java:702)
	at org.apache.hadoop.util.Shell.execCommand(Shell.java:791)
	at org.apache.hadoop.util.Shell.execCommand(Shell.java:774)
	at org.apache.hadoop.fs.RawLocalFileSystem.setPermission(RawLocalFileSystem.java:646)
	at org.apache.hadoop.fs.FilterFileSystem.setPermission(FilterFileSystem.java:472)
	at org.apache.hadoop.fs.ChecksumFileSystem.create(ChecksumFileSystem.java:460)
	at org.apache.hadoop.fs.ChecksumFileSystem.create(ChecksumFileSystem.java:426)
	at org.apache.hadoop.fs.FileSystem.create(FileSystem.java:906)
	at org.apache.hadoop.fs.FileSystem.create(FileSystem.java:887)
	at org.apache.hadoop.fs.FileSystem.create(FileSystem.java:784)
	at org.apache.hadoop.fs.FileUtil.copy(FileUtil.java:365)
	at org.apache.hadoop.fs.FileUtil.copy(FileUtil.java:338)
	at org.apache.hadoop.fs.FileUtil.copy(FileUtil.java:289)
	at org.apache.hadoop.fs.FileSystem.copyToLocalFile(FileSystem.java:1968)
	at org.apache.hadoop.fs.FileSystem.copyToLocalFile(FileSystem.java:1937)
	at org.apache.hadoop.fs.FileSystem.copyToLocalFile(FileSystem.java:1913)
	at com.beifeng.TestCopy.myCopyToLocal(TestCopy.java:44)
	at com.beifeng.TestCopy.main(TestCopy.java:15)
```

### 解决：

```
//fs.copyToLocalFile(new Path("/hadoop/put/111.txt"), new Path("e:/txt/copyFormHDFS.txt"));
		fs.copyToLocalFile(false, new Path("/hadoop/put/111.txt"), new Path("e:/txt/copyFormHDFS.txt"),true);
```

