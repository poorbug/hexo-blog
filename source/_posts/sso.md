---
title: SSO 踩坑记
date: 2017-09-27
tags:
- SSO
- Single Sign On
- 单点登录
---

SSO, Single Sign On, 单点登录。

## 作用
在一个站点登录后，该站的所有关联站点都可免登录。例如在淘宝登录后，天猫也是已登录状态。当然，在某个站点登出后，其他站点也都登出。

## 原理
创建一个单独的 SSO 工程用来维护登录状态。
以 www.poorbug.cn(以下简写为 www ) 与 sso.poorbug.cn(以下简写为 sso ) 为例

1. 用户访问 server 下要求登录的 api 时
2. server 检测是否登录
	1. 如果已登录则校验 token 是否过期
		1. 未过期则继续访问
		2. 已过期则执行 第 3 步
	2. 如果未登录则执行 第 3 步
3. server 将请求 `redirect` 到 sso
4. 用户授权登录后 server `redirect` 到 `/logined` 
5. 在 `/logined` 下存储 `token` 等信息
6. `/logined` 将请求 `redirect` 到相应页面
7. 已登录状态成功访问页面

## 坑
1. api 应该两部分，需要登录与不需要登录
2. 通过地址栏 url 访问的地址才可 redirect，ajax 请求会因为跨域导致无法 redirect
