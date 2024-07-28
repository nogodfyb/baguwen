SpringBoot内部配置文件的加载顺序，从高到低大致如下：
### `bootstrap.yml`（或`bootstrap.properties`）：
这个文件用于应用程序上下文的引导阶段配置，如配置应用程序的外部配置源（如配置中心）等。它的加载时机早于`application.yml`或`application.properties`，且其配置内容不会被后者覆盖。
### 项目根目录下的`**config**`文件夹中的配置文件：
包括`application.properties`、`application.yml`、`application-{profile}.properties`、`application-{profile}.yml`等，这些文件的优先级高于项目根目录下的配置文件。
### 项目根目录下的配置文件：
包括`application.properties`、`application.yml`、`application-{profile}.properties`、`application-{profile}.yml`等。
### `**resources/config**`目录下的配置文件：
这是放置在项目的资源（`resources`）目录下的`config`文件夹中的配置文件，优先级低于项目根目录及其`config`文件夹下的配置文件。
### `**resources**`目录下的配置文件：
最后加载的是项目资源目录（`resources`）下的配置文件，如`application.properties`、`application.yml`等。
### @PropertySource注解
如果@Configuration类上使用了@PropertySource注解来指定额外的配置文件，那么这些文件将在上述所有内部配置文件之后加载。

如果同一配置项在多个配置文件中出现，那么高优先级的配置文件中的值将覆盖低优先级配置文件中的值。此外，如果项目中同时存在`application.properties`和`application.yml`文件，并且它们位于同一优先级目录下，那么`application.properties`文件将首先被加载，但其内容可能会被随后加载的`application.yml`文件中的相同配置项所覆盖（具体取决于它们的加载顺序和配置项是否冲突）。然而，这种情况通常建议避免，以保持配置文件的清晰和一致性。
