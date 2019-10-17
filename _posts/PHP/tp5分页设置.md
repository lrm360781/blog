---
title: tp5设置分页
date: 2019-01-15 12:03:10
tags:
    - tp
    - 分页
---

分页设置
==
## controller层代码

```yaml
//查询状态为1的用户数据，并且每页显示10数据
$list = User::where('status',1)->paginate(10);
//把分页数据赋值给模板变量list
$this->assign('list',$list);

return $this->fetch();

```
## view层代码

```yaml
<div>
    <ul>
        {volist name='list' id='user'}
          <li>{$user.name}</li>
        {/volist}
    </ul>
</div>
{$list->render()}
```

用上面代码需要引入bootstarp样式
```yaml
<!-- 最新版本的 Bootstrap 核心 CSS 文件 -->
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@3.3.7/dist/css/bootstrap.min.css" integrity="sha384-BVYiiSIFeK1dGmJRAkycuHAHRg32OmUcww7on3RYdg4Va+PmSTsz/K68vbdEjh4u" crossorigin="anonymous">
```


