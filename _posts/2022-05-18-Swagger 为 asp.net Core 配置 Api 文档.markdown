---
layout:     post
title:      "Swagger 为 asp.net Core 配置 Api 文档"
subtitle:   "Swagger 为 asp.net Core 配置 Api 文档"
date:       2022-05-18 10:00
author:     "ZXZ"
header-style:   "text"
header-img: "img/post-bg-rwd.jpg"
tags:
    - Swagger
    - asp.net Core
---

# Swagger 为 asp.net Core 配置 Api 文档

1、安装 NuGet 插件

有些教程只提示安装 "Swashbuckle.AspNetCore.Swagger "

有的教程提示安装 "Swashbuckle.AspNetCore.SwaggerGen " 和 "Swashbuckle.AspNetCore.SwaggerUl "

我这里都安装了。

![img](/img/4-Swagger/I4KNFFL16IP$N_BNFNZUNWF-16500027176281-16500027189862.png)

2、在 Startup 中注册 

我这里先封装成了一个类

```C#
 public static class SetupSwagger
    {
        public static void AddSwaggerSetup(this IServiceCollection services)
        {
            if (services == null) throw new ArgumentNullException(nameof(services));
            var ApiName = "WebApi.Core";

            services.AddSwaggerGen(c =>
            {
                c.SwaggerDoc("V1", new OpenApiInfo
                {
                    //  { ApiName } 定义成全局变量,方便修改
                    Version = "V1",
                    Title = $"{ApiName} 接口文档--Netcorv1e 3.1",
                    Description = $"{ApiName} HTTP API V1",
                });

                c.OrderActionsBy(o=>o.RelativePath);

                //获取xml注释文件的目录
                //生成时会自动生成一个 项目名称.xml 文件
                var xmlPath = Path.Combine(AppContext.BaseDirectory, "JingKongGuanLiServers.xml");
                c.IncludeXmlComments(xmlPath,true);
            });
        }
    }
```

然后在 Startup 类 -> ConfigureServices 中注册

```c#
 		// This method gets called by the runtime. Use this method to add services to the container.
        public void ConfigureServices(IServiceCollection services)
        {
            //注册 Swagger api帮助文档
            services.AddSwaggerSetup();
            
            services.AddControllers();
        }
```

3、 在 Startup 类 -> Configure 管道中配置中间件

```
		// This method gets called by the runtime. Use this method to configure the HTTP request pipeline.
        public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
        {
            if (env.IsDevelopment())
            {
                app.UseDeveloperExceptionPage();
            }
            
            
            //在管道（pipline）中添加Swagger（中间件）,及其路径配置
            app.UseSwagger();
            app.UseSwaggerUI(s =>
            {
                s.SwaggerEndpoint($"/swagger/V1/swagger.json", "WebApi.Core V1");
                s.RoutePrefix = "ApiDoc";
            });
            

            app.UseHttpsRedirection();

            //启动路由中间件
            app.UseRouting();

            app.UseAuthorization();

            app.UseEndpoints(endpoints =>
            {
                endpoints.MapControllers();
            });
        }
```

4、把启动页设置为配置的文档页面

![image-20220415141246067](/img/4-Swagger/image-20220415141246067-16500031677443.png)

# 遇到过的错误：

![image-20220415141457992](/img/4-Swagger/image-20220415141457992-16500032995945.png)

**解决办法**：

​	检查API接口方法的请求方式是否书写！

![image-20220415141543433](/img/4-Swagger/image-20220415141543433-16500033450186.png)

