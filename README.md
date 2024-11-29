
目录* [前言](https://github.com)
* [一、授权概述](https://github.com)
* [二、用户权限](https://github.com)
* [三、用户授权流程](https://github.com):[FlowerCloud机场节点订阅](https://dahelaoshi.com)
* [三、Spring Security授权方式](https://github.com)
	+ [1、请求级别授权](https://github.com)
	+ [2、方法级别授权](https://github.com)

# 前言


本文介绍什么是授权，Spring Security的授权配置有哪些，配合以下内容观看效果更佳！！！


* 什么是授权，授权有哪些流程，Spring Security的授权配置有几种？请查看九、Spring Boot集成Spring Security之授权概述
* HTTP请求授权的实现原理是什么，如何配置HTTP请求授权？请查看十、Spring Boot集成Spring Security之HTTP请求授权
* 方法授权的实现原理是什么，如何配置方法授权？请查看十一、Spring Boot集成Spring Security之方法授权
* 如何实现基于RBAC模型的授权方式？请查看十二、Spring Boot集成Spring Security之基于RBAC模型的授权


# 一、授权概述


​ 授权简单来说就是判断某个用户能不能访问某个接口，可以访问时授权成功，不能访问时授权失败；用户包括已登录的用户和未登录的用户即匿名用户，接口包括接口地址和接口的请求类型，接口对于系统使用者来说可以简单理解为菜单按钮。


​ 目前最流行的权限模型是RBAC权限模型，这种模型的思想是将菜单/接口/权限按照完成某项任务的最小权限进行分组，分出来的组即为角色，再按照用户的职责授予相应的角色。其中菜单/接口/权限和角色之间为多对多的关系，即一项任务可能需要多个操作或者多项任务可能需要同一个操作；用户与角色之间也是多对多的关系，即一个用户可能需要完成多项任务或者不同用户可能需要完成相同的任务。


​ Spring Security的授权还包括对认证结果、接口请求参数、接口返回值等更细粒度的处理。


# 二、用户权限


​ Spring Security中用户的权限接口为GrantedAuthority，并提供默认实现SimpleGrantedAuthority，SimpleGrantedAuthority有一个String属性role，role用于判断用户是否允许访问接口。


​ 在配置接口权限时还有两个权限的概念role（hasXxxRole）和authority（hasXxxAuthority），role和authority最终都会转化为SimpleGrantedAuthority的role属性，并和用户的权限作对比，以判断用户是否允许访问接口，唯一的区别是role转为SimpleGrantedAuthority的role属性时会默认添加`ROLE_`前缀，而authority会直接转化为SimpleGrantedAuthority的role属性，即ROLE\_{role}\={authority}\=simpleGrantedAuthority.role。


# 三、用户授权流程


1. 认证时设置用户权限
2. 授权时获取接口及其需要的权限
3. 校验用户权限和接口权限是否有交集
4. 有交集时校验成功调用接口
5. 没有交集时校验失败抛出异常


# 三、Spring Security授权方式


## 1、请求级别授权


1. 实现方式：过滤器
2. 适用场景
	* 基于URL的访问控制
	* 统一的安全策略
	* 简单地权限控制


## 2、方法级别授权


1. 实现方式：AOP（拦截器）
2. 适用场景
	* 细粒度的访问控制
	* 动态权限检查
	* 基于业务逻辑的权限控制


