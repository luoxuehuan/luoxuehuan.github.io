##　1.web.xml中作如下配置：

```
	<!-- Jersey REST spring集成 -->
	<servlet>
		<servlet-name>Jersey REST Service</servlet-name>
		<servlet-class>com.sun.jersey.spi.spring.container.servlet.SpringServlet</servlet-class>
		<!-- rest服务路径 -->
		<init-param>
			<param-name>com.sun.jersey.config.property.packages</param-name>
			<param-value>cn.com.hadon</param-value>
		</init-param>
		<!-- 开启POJO和JSON的支持 -->
		<init-param>
			<param-name>com.sun.jersey.api.json.POJOMappingFeature</param-name>
			<param-value>true</param-value>
		</init-param>
		<!-- 开启POST方法与DELETE方法转换 -->
		<init-param>
			<param-name>com.sun.jersey.spi.container.ContainerRequestFilters</param-name>
			<param-value>com.sun.jersey.api.container.filter.PostReplaceFilter</param-value>
		</init-param>
	</servlet>
	<servlet-mapping>
		<servlet-name>Jersey REST Service</servlet-name>
		<url-pattern>/rest/*</url-pattern>
	</servlet-mapping>
```


##　2.编写代码：

```
import java.text.ParseException;
import java.util.ArrayList;
import java.util.List;

import javax.ws.rs.GET;
import javax.ws.rs.Path;
import javax.ws.rs.PathParam;
import javax.ws.rs.Produces;
import javax.ws.rs.core.MediaType;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

@Service
@Path("/ResttestService")
public class restservice {



	// http://localhost:8080/workteam/rest/ResttestService/testget/pcode/teststr

	/**
	 * POST（CREATE）：在服务器新建一个资源。
	 */
	@POST
	@Path("/testpost/{projectcode}/{testcode}")
	@Produces({ MediaType.APPLICATION_JSON })
	public @ResponseBody Map testpost(@PathParam("projectcode") String projectcode,
			@PathParam("testcode") String testcode) {
		Map map = new HashMap();
		String message = "";
		/* 测试能否拿到传递过来的参数 */
		System.out.println("projectcode:" + projectcode + "testcode:" + testcode);

		/**
		 * 执行取资源操作,省略~
		 */
		map.put("projectcode", "testcode");
		/* 返回给前台 */
		return map;
	}

	/**
	 * DELETE（DELETE）：从服务器删除资源
	 */
	@DELETE
	@Path("/testdelete/{projectcode}/{testcode}")
	@Produces({ MediaType.APPLICATION_JSON })
	public @ResponseBody Map testdelete(@PathParam("projectcode") String projectcode,
			@PathParam("testcode") String testcode) {
		Map map = new HashMap();
		String message = "";
		/* 测试能否拿到传递过来的参数 */
		System.out.println("projectcode:" + projectcode + "testcode:" + testcode);

		/**
		 * 执行删除资源操作,省略~
		 */
		map.put("projectcode", "testcode");

		/* 返回给前台是否操作成功 */
		return map;
	}

	/**
	 * PUT（UPDATE）：在服务器更新资源（客户端提供改变后的完整资源）。
	 */
	@PUT
	@Path("/testput/{projectcode}/{testcode}")
	@Produces({ MediaType.APPLICATION_JSON })
	public @ResponseBody Map testput(@PathParam("projectcode") String projectcode,
			@PathParam("testcode") String testcode) {

		/* 测试能否拿到传递过来的参数 */
		System.out.println("projectcode:" + projectcode + "testcode:" + testcode);
		Map map = new HashMap();
		/**
		 * 执行取资源操作,省略~
		 */
		map.put("projectcode", "testcode");
		/* 将取得的资源返回给前台 */
		return map;
	}

	/**
	 * GET（SELECT）：从服务器取出资源（一项或多项）
	 * 
	 * 可以在浏览器里直接输入路径进行测试
	 * 
	 * @param projectcode
	 * @param testcode
	 */
	@GET
	@Path("/testget/{projectcode}/{testcode}")
	@Produces({ MediaType.APPLICATION_JSON })
	public @ResponseBody Map testget(@PathParam("projectcode") String projectcode,
			@PathParam("testcode") String testcode) {

		/* 测试能否拿到传递过来的参数 */
		System.out.println("projectcode:" + projectcode + "testcode:" + testcode);
		Map map = new HashMap();
		/**
		 * 执行取资源操作,省略~
		 */
		map.put("projectcode", "testcode");
		/* 将取得的资源返回给前台 */
		return map;
	}

}
```
##　３.测试

１.利用火狐的ＨｔｔｐＲｅｑｕｅｓｔｅｒ插件进行测试

![这里写图片描述](http://img.blog.csdn.net/20160512161550492)

分别执行ＧＥＴ　ＰＯＳＴ　ＰＵＴ　ＤＥＬＥＴＥ方法。
对应的ｕｒｌ为：
GET http://localhost:8080/workteam/rest/ResttestService/testget/pcode/teststr
PUT http://localhost:8080/workteam/rest/ResttestService/testput/pcode/teststr
POST http://localhost:8080/workteam/rest/ResttestService/testpost/pcode/teststr
DELETE http://localhost:8080/workteam/rest/ResttestService/testdelete/pcode/teststr
在右侧可以看到都可以拿到后台的返回值。
且控制台也可以看到　方法拿到了前台传递的值。
![这里写图片描述](http://img.blog.csdn.net/20160512161830917)