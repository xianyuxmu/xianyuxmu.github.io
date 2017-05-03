---
layout: post
title: Field and Domain
date: 2017-05-03 19:32:00 +0800
categories:
- Tech
tags:
- Software Design

---

## Definition of FIELD

- an open land area free of woods and buildings.
- an area of land marked by the presence of particular objects or features.

from ***Merriam-Webster***


## Definition of DOMAIN

- in law: complete and absolute (see absolute 3) ownership of land.
	- *our highways and roads have been in the domain of state and local governments — T. H. White b. 1915*
- a region distinctively marked by some physical feature a domain of rushing streams, tall trees, and lakes.

from ***Merriam-Webster***

----

## Field 和 Domain 的中文释义

我们由英文释义可以知道，在中文，`Field` 应该翻译成 `域`，而 `Domain` 应翻译成 `领域`。

## 软件中的域和领域

在软件的架构中，我们常常会接触到域设计、领域设计。

### 领域设计

领域设计(Domain)是最常见的，在领域驱动软件设计(Domain-Driven Design, DDD)中，领域对象是核心，每个领域对象都是一个相对完整的内聚的业务对象描述。

### 域设计

域设计(Field Design) 更加倾向于一定规则描述下的范围。一个域中可能会包含一个或多个的领域(Domain)。


## Field 和 Domain 两者的关系

领域是对一个完整的范围的分割，领域是内聚的、相互之间无交集的。全球的域名系统、IP分配都是很典型领域设计的例子。

领域是严格条件下的域。

----

Robin on May 03, 2017 

