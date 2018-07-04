>出错情况：

	在cmd中运行scala提示错误：找不到或无法加载主类 scala.tools.nsc.MainGenericRunner

>出错原因：

	scala是直接拷贝的其他电脑上的scala,放在Program File文件夹里。scala安装目录出现了空格。

>问题解决：

	讲Scala 移动到没有空格的文件夹下。重新设置SCALA_HOME。文件解决。