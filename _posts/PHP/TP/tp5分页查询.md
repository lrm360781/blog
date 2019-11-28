---
title: tp5分页查询
date: 2019-02-20 21:14:13
tags:
    - tp
    - MySQL
    - 分页
---
分页设置
==

controller层代码
--

```php
//查询状态为1的用户数据，并且每页显示10数据
$list = User::where('status',1)->paginate(10);
//把分页数据赋值给模板变量list
$this->assign('list',$list);

return $this->fetch();

```

view层代码
--
```html
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
```html
<!-- 最新版本的 Bootstrap 核心 CSS 文件 -->
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@3.3.7/dist/css/bootstrap.min.css" integrity="sha384-BVYiiSIFeK1dGmJRAkycuHAHRg32OmUcww7on3RYdg4Va+PmSTsz/K68vbdEjh4u" crossorigin="anonymous">
```

多表查询、分页
===
以主表article查询，起别名a，，联合表art_category，起别名c，条件是id等同，用join

field查询需要的字段，

where条件查询

order排序差

分页：每页10条
```php
    //多表联合查询
    $data = Db::name('article')
        ->alias('a')
        ->join('art_category c','a.category_id = c.category_id')
        ->field('a.art_id,a.imageurl,a.title,a.art_desc,a.category_pid,a.create_time,c.category_id,c.category_name')
        ->where('a.status',1)
        ->order('a.art_id','desc')
        ->paginate(10);
 
    //多表联合查询-内连接
    $articleList = Db::name("tp_art_rela_category")
        ->alias("arc")
        ->join('tp_article a','a.art_id = arc.art_id','inner')
        ->where('arc.category_id',$category_id)
        ->select();
        
    $page = $data->render();
        //输出数据
        $this -> assign('data',$data);
        //输出分页
        $this -> assign('page',$page);
            
        return $this->fetch();
```

数据输出
--
```html
<div class="item-box-rt flex" style="">
	<div class="item-content">
		<a class="item-title" href="{:url('detail/detail',['type'=>99,'id'=>$vo['art_id']])}">{$vo.title}</a>
		<div class="sm-hidden md-hidden item-desc">
	      <a href="{:url('detail/detail',['type'=>99,'id'=>$vo['art_id']])}">{$vo.art_desc}</a>
		</div>
	</div>
	<div class="item-info flex text-overflow">
        <p>
            <i class="fa 
              {switch name='$vo.category_pid'}
                {case value='1'}fa-jsfiddle{/case}
                {case value='4'}fa-download{/case}
                {default /}fa-download
              {/switch}
              "style="font-size: 1.2rem;color:#e2712c;" aria-hidden="true" >
            </i>    
            {$vo.category_pid}      
            <span>
                {switch name='$vo.category_pid'}
                    {case value='1'}vv{/case}
                    {case value='4'}44{/case}
                    {default /}其他
                {/switch}
            </span>
            id:{$vo.category_id}
        </p>
        <p>
          <i class="fa fa-clock-o" aria-hidden="true"></i>
          {$vo.create_time|date="Y-m-d",###}
        </p>
        <p><i class="fa fa-heart-o" aria-hidden="true"></i>321</p>
        <p><i class="fa fa-download" aria-hidden="true"></i><a href="#">下载 </a></p>
    </div>
</div>
```

页面标识
--
```html
<div class="pagination-box">{$page}</div>
```


