### 1. 配置 Spring MVC 以支持文件上传
首先，需要在 Spring 配置文件中添加对文件上传的支持。你可以在 Spring 的 Java 配置类或 XML 配置文件中进行配置。
#### Java 配置类方式
```
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.multipart.commons.CommonsMultipartResolver;

@Configuration
public class AppConfig {

    @Bean
    public CommonsMultipartResolver multipartResolver() {
        CommonsMultipartResolver multipartResolver = new CommonsMultipartResolver();
        multipartResolver.setMaxUploadSize(50000000); // 设置最大上传文件大小为 50MB
        return multipartResolver;
    }
}
```
#### XML 配置文件方式
```
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
                           http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
        <property name="maxUploadSize" value="50000000"/> <!-- 设置最大上传文件大小为 50MB -->
    </bean>

</beans>
```
### 2. 创建一个表单用于文件上传
创建一个包含文件上传字段的 HTML 表单。注意，表单的 `enctype` 属性必须设置为 `multipart/form-data`。
```
<!DOCTYPE html>
<html>
<head>
    <title>File Upload</title>
</head>
<body>
    <form method="post" action="/upload" enctype="multipart/form-data">
        <input type="file" name="file" />
        <input type="submit" value="Upload" />
    </form>
</body>
</html>
```
### 3. 编写控制器方法来处理文件上传请求
在控制器中编写处理文件上传请求的方法。使用 `@RequestParam` 注解将上传的文件绑定到方法参数上。
```
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.multipart.MultipartFile;
import org.springframework.web.bind.annotation.ResponseBody;

import java.io.File;
import java.io.IOException;

@Controller
public class FileUploadController {

    @PostMapping("/upload")
    @ResponseBody
    public String handleFileUpload(@RequestParam("file") MultipartFile file) {
        if (!file.isEmpty()) {
            try {
                // 获取文件名
                String fileName = file.getOriginalFilename();
                // 将文件保存到指定路径
                String filePath = "C:/uploads/" + fileName;
                File dest = new File(filePath);
                file.transferTo(dest);
                return "File uploaded successfully: " + fileName;
            } catch (IOException e) {
                e.printStackTrace();
                return "Failed to upload file: " + e.getMessage();
            }
        } else {
            return "Failed to upload file: File is empty.";
        }
    }
}
```
