JettyEmbedSpringMVC
===================

Jetty Embed sprintmvc

visit website:http://localhost:8000/index
# 遇到问题汇总 #
1. org.apache.jasper.JasperException: PWC6345: There is an error in invoking javac.  A full JDK (not just JRE) is required
	解决办法：http://www.oschina.net/question/12_8224 此问题的原因就是运行该程序的电脑上面的java.exe有path目录中有多个，但jdk\bin\java.exe在命令行执行时不是第一个找到，可以将其它的java.exe放在xxx用户变量下面的path中，将jdk\bin\java.exe放在系统变量中的path中
2. org.apache.jasper.JasperException: java.err.nojdk
	解决办法：在pom.xml>project>build>plugins中添加
			`<plugin>
				<groupId>org.mortbay.jetty</groupId>
				<artifactId>jetty-maven-plugin</artifactId>
				<version>${jetty.version}</version>
				<configuration>
					<scanIntervalSeconds>10</scanIntervalSeconds>
					<systemProperties>
					<!-- 解决报错信息为 java.err.nojdk 的问题					
						http://dugu108.iteye.com/blog/1773629
						-->
						<systemProperty>
							<name>org.apache.jasper.compiler.disablejsr199</name>
							<value>true</value>
						</systemProperty>
					</systemProperties>
					<webAppConfig>
						<contextPath>/vem</contextPath>
					</webAppConfig>
				</configuration>
			</plugin>`
3. javax.el.ExpressionFactory.newInstance()Ljavax/el/ExpressionFactory; 原因是jsp-api的版本是要2.2以上 `<dependency>
			<groupId>javax.servlet.jsp</groupId>
			<artifactId>jsp-api</artifactId>
			<version>2.2</version>
			<scope>provided</scope>
		</dependency>`
4. The absolute uri: http://java.sun.com/jsp/jstl/fmt cannot be resolved in either web.xml or the jar 解决办法http://hi.baidu.com/pianistsoftwar/item/1d5bc1f2ee4ee112cf9f32fb 
  将standard.jar中的*.tld文件放至WEB-INF目录下面，同时在web.xml中添加入面的的配置`<jsp-config>
   <taglib>
    <taglib-uri>http://java.sun.com/jsp/jstl/core</taglib-uri>
    <taglib-location>/WEB-INF/c.tld</taglib-location>
   </taglib>
   <taglib>
    <taglib-uri>http://java.sun.com/jsp/jstl/xml</taglib-uri>
    <taglib-location>/WEB-INF/x.tld</taglib-location>
   </taglib>
   <taglib>
    <taglib-uri>http://java.sun.com/jsp/jstl/fmt</taglib-uri>
    <taglib-location>/WEB-INF/fmt.tld</taglib-location>
   </taglib>
   <taglib>
    <taglib-uri>http://java.sun.com/jsp/jstl/sql</taglib-uri>
    <taglib-location>/WEB-INF/sql.tld</taglib-location>
   </taglib>
</jsp-config>`
  方案二：将解压出来的*.tld 放在web-inf/tld目录下面




# 待实现功能 #


1. 热部署 http://wiki.eclipse.org/Jetty/Feature/Hot_Deployment
2. JRebel+jetty+maven