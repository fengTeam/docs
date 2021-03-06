{% extends "./leanstorage_guide.tmpl" %}

{# --Start--变量定义，主模板使用的单词，短语所有子模板都必须赋值 #}

{% set productName ="LeanStorage" %}
{% set platform_title ="PHP" %}
{% set segment_code = platform_title | lower %}
{% set sdk_name = platform_title + " SDK" %}
{% set baseObjectName ="LeanObject" %}
{% set objectIdName ="objectId" %}
{% set updatedAtName ="updatedAt" %}
{% set createdAtName ="createdAt" %}
{% set backgroundFunctionTemplate ="xxxxInBackground" %}
{% set saveEventuallyName ="saveEventually" %}
{% set deleteEventuallyName ="deleteEventually" %}
{% set relationObjectName ="LeanRelation" %}
{% set pointerObjectName ="LeanPointer" %}
{% set baseQueryClassName ="LeanQuery" %}
{% set geoPointObjectName ="GeoPoint" %}
{% set userObjectName ="LeanUser" %}
{% set fileObjectName ="LeanFile" %}
{% set dateType= "DateTime" %}
{% set byteType= "LeanBytes" %}
{% set funtionName_whereKeyHasPrefix = "startsWith()" %}
{% set saveOptions_query= "where" %}
{% set saveOptions_fetchWhenSave= "fetch_when_save" %}
{% set link_to_acl_doc = "（PHP 文档待补充）" %}
{% set link_to_sms_guide_doc = "（PHP 文档待补充）" %}

{# --End--变量定义，主模板使用的单词，短语的定义所有子模板都必须赋值 #}

{# --Start--主模板留空的代码段落，子模板根据自身实际功能给予实现 #}

{% block code_create_todo_object %}
```php
// "Todo" 对应的就是控制台中的 Class Name
$todo = new LeanObject("Todo");

// 或者
$todo = LeanObject::create("Todo");
```
{% endblock %}

{% block code_quick_save_a_todo %}

```php
$todo = new LeanObject("Todo");
$todo->set("title", "工程师周会");
$todo->set("content", "每周工程师会议，周一下午2点");
try {
    $todo->save();
    // 存储成功
} catch (CloudException $ex) {
    // 失败的话，请检查网络环境以及 SDK 配置是否正确
}
```
{% endblock %}

{% block text_and_link_to_install_doc %}
请阅读 [{{platform_title}} 安装指南](sdk_setup-php.html)。
{% endblock %}

{% block code_save_object_by_cql %}

```php
// 执行 CQL 语句实现新增一个 TodoFolder 对象
try {
    $result = LeanQuery::doCloudQuery("INSERT INTO TodoFolder(name, priority) values('工作', 1)");
    // 保存成功
} catch (CloudException $ex) {
    // 保存失败
}
```

{% endblock %}

{% block section_saveOptions %}{% endblock %}
{% block code_saveoption_query_example %}

```php
// PHP 有待支持
```
{% endblock %}


{% block code_quick_save_a_todo_with_location %}

```php
$todo = new LeanObject("Todo");
$todo->set("title", "工程师周会");
$todo->set("content", "每周工程师会议，周一下午2点");
$todo->set("location", "会议室"); // 只要添加这一行代码，服务端就会自动添加这个字段
try {
    $todo->save();
    // 存储成功
} catch (CloudException $ex) {
    // 失败的话，请检查网络环境以及 SDK 配置是否正确
}
```

{% endblock %}

{% block code_save_todo_folder %}

```php
$todoFolder = new LeanObject("TodoFolder"); // 构建对象
$todoFolder->set("name", "工作");           // 设置名称
$todoFolder->set("priority", 1);            // 设置优先级

try {
    $todoFolder->save();
    // 保存到服务端
} catch (CloudException $ex) {
    // 失败
}
```
{% endblock %}

{% block code_get_todo_by_objectId %}

```php
$query = new LeanQuery("Todo");
$todo  = $query->get("558e20cbe4b060308e3eb36c");
// $todo 就是 ID 为 558e20cbe4b060308e3eb36c 的对象实例
```
{% endblock %}

{% block code_save_callback_get_objectId %}

```php
$todo = new LeanObject("Todo");
$todo->set("title", "工程师周会");
$todo->set("content", "每周工程师会议，周一下午2点");
$todo->set("location", "会议室"); // 只要添加这一行代码，服务端就会自动添加这个字段
try {
    $todo->save();
    // 保存成功之后，objectId 会自动加载到对象
    $todo->getObjectId();
} catch (CloudException $ex) {
    // 失败的话，请检查网络环境以及 SDK 配置是否正确
}
```
{% endblock %}

{% block code_access_todo_folder_properties %}

```php
$query = new LeanQuery("Todo");
$todo  = $query->get("558e20cbe4b060308e3eb36c");
// $todo 就是 ID 为 558e20cbe4b060308e3eb36c 的对象实例

$todo->get("location");
$todo->get("title");
$todo->get("content");

// 获取三个特殊属性
$todo->getObjectId();
$todo->getUpdatedAt();
$todo->getCreatedAt();

```
{% endblock %}

{% block text_refresh_object%}{% endblock %}

{% block code_fetch_todo_by_objectId %}
```php
// 假如已知了 objectId 可以用如下的方式构建一个 LeanObject
$todo = LeanObject::create("Todo", "5656e37660b2febec4b35ed7");
// 然后调用刷新的方法，将数据提取到对象
$todo->fetch();
```
{% endblock %}

{% block code_update_object_by_cql %}

```php
// 执行 CQL 语句实现更新一个 TodoFolder 对象
LeanCloud::doCloudquery("update TodoFolder set name='家庭' where objectId='558e20cbe4b060308e3eb36c'");
```
{% endblock %}

{% block code_object_fetchWhenSave %}

```
// PHP 有待支持
```
{% endblock %}

{% block code_object_fetch_with_keys %}

```php
// PHP 有待支持
```
{% endblock %}

{% block code_update_todo_location %}

```php
$todo = new LeanObject("Todo");
$todo->set("title", "工程师周会");
$todo->set("content", "每周工程师会议，周一下午2点");
$todo->set("location", "会议室");
try {
    $todo->save();
    $todo->set("location", "二楼大会议室");
    $todo->save();
} catch Exception $ex {
    // 失败的话，请检查网络环境以及 SDK 配置是否正确
}
```
{% endblock %}

{% block code_update_todo_content_with_objectId %}

```php
// 参数依次为 className、objectId
$todo = LeanObject::create("Todo", "558e20cbe4b060308e3eb36c");

// 修改 content
$todo->set("content","每周工程师会议，本周改为周三下午3点半。");
// 保存到云端
$todo->save();
```

{% endblock %}

{% block code_atomic_operation_increment %}

```php
$theTodo = LeanObject::create("Todo", "564d7031e4b057f4f3006ad1");
$theTodo->set("views", 0); //初始值为 0

$theTodo->increment("views", 5); // 原子增加查看的次数
$theTodo->save();

$theTodo->increment("views"); // 默认加 1
$theTodo->save();
```

{% endblock %}

{% block code_atomic_operation_array %}

* `addIn()`<br>
  将指定对象附加到数组末尾。
* `addUniqueIn()`<br>
  如果不确定某个对象是否已包含在数组字段中，可以使用此操作来添加。
* `removeIn()`<br>
  从数组字段中删除指定对象的所有实例。

{% endblock %}

{% block code_set_array_value %}

```php
$reminder1 = new \DateTime("2015-11-11 07:10:00");
$reminder2 = new \DateTime("2015-11-11 07:20:00");
$reminder3 = new \DateTime("2015-11-11 07:30:00");

$todo = new LeanObject("Todo");
$todo->addUniqueIn("reminders", $reminder1);
$todo->addUniqueIn("reminders", $reminder2);
$todo->addUniqueIn("reminders", $reminder3);
```

{% endblock %}

{% block code_batch_operation %}

```php
// 批量创建、更新
LeanObject::saveAll()

// 批量删除
LeanObject::destroyAll()

// 批量获取
LeanObject::fetchAll()
```
{% endblock %}

{% block code_batch_set_todo_completed %}

```php
$query = new LeanQuery("Todo");
$todos = $query->find();

forEach ($todos as $todo) {
    $todo->set("status", 1);
}
try {
    LeanObject::saveAll($todos);
    // 保存成功
} catch (CloudException $ex) {
    // 保存失败
}
```

{% endblock %}

{% block code_delete_todo_by_objectId %}

```php
$todo->destroy();
```

{% endblock %}

{% block code_relation_todoFolder_one_to_many_todo %}

```php
$todoFolder = new LeanObject("TodoFolder"); // 构建对象
$todoFolder->set("name", "工作");
$todoFolder->set("priority", 1);

$todo1 = new LeanObject("Todo");
$todo1->set("title", "工程师周会");
$todo1->set("content", "每周工程师会议，周一下午2点");
$todo1->set("location", "会议室");

$todo2 = new LeanObject("Todo");
$todo2->set("title", "维护文档");
$todo2->set("content", "每天 16：00 到 18：00 定期维护文档");
$todo2->set("location", "当前工位");

$todo3 = new LeanObject("Todo");
$todo3->set("title", "发布 SDK");
$todo3->set("content", "每周一下午 15：00");
$todo3->set("location", "SA 工位");

$relation = $todoFolder->getRelation("containedTodos"); // 新建一个 Relation
$relation->add($todo1);
$relation->add($todo2);
$relation->add($todo3);
// 上述 3 行代码表示 relation 关联了 3 个 Todo 对象

$todoFolder->save();
```
{% endblock %}

{% block code_pointer_comment_one_to_many_todoFolder %}

```php
$comment = new LeanObject("Comment"); // 构建 Comment 对象
$comment->set("like", 1); // 如果点了赞就是 1，而点了不喜欢则为 -1，没有做任何操作就是默认的 0
$comment->set("content", "这个太赞了！楼主，我也要这些游戏，咱们团购么？"); // 留言的内容

// 假设已知了被分享的该 TodoFolder 的 objectId 是 5590cdfde4b00f7adb5860c8
$comment->set("targetTodoFolder", LeanObject::create("TodoFolder", "5590cdfde4b00f7adb5860c8"));
// 以上代码的执行结果就会在 comment 对象上有一个名为 targetTodoFolder 属性，它是一个 Pointer 类型，指向 objectId 为 5590cdfde4b00f7adb5860c8 的 TodoFolder
```

{% endblock %}

{% block code_data_type %}
```php
$bool       = true;
$number     = 2015;
$string     = $number . " 年度音乐排行";
$date       = new \DateTime();
$bytesArray = LeanBytes::createFromBase64Data(base64_encode("Hello world!"));

$testObject = new LeanObject("DataTypes");

$testObject->set("testBoolean", $bool);
$testObject->set("testInteger", $number);
$testObject->set("testString", $string);
$testObject->set("testData", $bytesArray);
$testObject->set("testDate", $date);
$testObject->set("testArray", array($string, $number));
$testObject->set("testAssociativeArray",
                 array("number" => $number,
                       "string" => $string));
$testObject->save();
```

此外，Array 和 Associative Array 支持嵌套，这样在一个 `LeanObject` 中就可以使用它们来储存更多的结构化数据。
{% endblock %}

{% block code_create_geoPoint %}

```php
$geopoint = new GeoPoint(39.9, 116.4);
```
{% endblock %}

{% block code_use_geoPoint %}

```php
$todo->set("whereCreated", $point);
```
{% endblock %}

{% block code_serialize_baseObject_to_string %}

```php
// PHP 暂不支持
```
{% endblock %}

{% block code_deserialize_string_to_baseObject %}

```php
// PHP 暂不支持
```
{% endblock %}

{% block code_data_protocol_save_date %}{% endblock %}

{% block code_create_avfile_by_stream_data %}

```php
$file = LeanFile::createWithData("resume.txt", "Working with LeanCloud is great!", "text/plain");
```

{% endblock %}

{% block code_create_avfile_from_local_path %}

```php
// 文件类型如果不提供，会按照结尾符自动识别
$file = LeanFile::createWithLocalFile("/tmp/LeanCloud.png");
```

{% endblock %}

{% block code_create_avfile_from_url %}

```php
$file = LeanFile::createWithUrl("test.gif", "http://ww3.sinaimg.cn/bmiddle/596b0666gw1ed70eavm5tg20bq06m7wi.gif");
```
{% endblock %}

{% block code_upload_file %}

```php
$file->save();
$file->getUrl(); // 返回一个唯一的 Url 地址
```

{% endblock %}

{% block code_upload_file_with_progress %}

```php
// PHP 不支持
```
{% endblock %}

{% block code_download_file %}

```php 
// PHP 不支持
```

{% endblock %}

{% block code_file_image_thumbnail %}

```php
$file = LeanFile::createWithUrl("test.jpg", "文件-url");
$file->getThumbnailUrl(100, 100);
```

{% endblock %}

{% block code_file_metadata %}
```php
$file = LeanFile::createWithLocalFile("/tmp/xxx.jpg");
$file->setMeta("width", 100);
$file->setMeta("height", 100);
$file->setMeta("author", "LeanCloud");
$file->save();
```
{% endblock %}

{% block code_file_delete %}

```php
$file->destroy();
```

{% endblock %}

{% block code_cache_operations_file %}{% endblock %}

{% block code_create_query_by_className %}

```php
$query = new LeanQuery("Todo");
```
{% endblock %}

{% block text_create_query_by_avobject %}{% endblock %}

{% block code_create_query_by_avobject %}

{% endblock %}

{% block code_priority_equalTo_zero_query %}

```php
$query = new LeanQuery("Todo");
$query->equalTo("priority", 0);
$todos = $query->find();
```
{% endblock %}

{% block code_priority_equalTo_zero_and_one_wrong_example %}

```php
$query = new LeanQuery("Todo");
$query->equalTo("priority", 0);
$query->equalTo("priority", 1);
// 如果这样写，第二个条件将覆盖第一个条件，查询只会返回 priority = 1 的结果
$todos = $query->find();
```
{% endblock %}

{% block table_logic_comparison_in_query %}
逻辑操作 | AVQuery 方法|
---|---
等于 | `equalTo`
不等于 |  `notEqualTo`
大于 | `greaterThan`
大于等于 | `greaterThanOrEqualTo`
小于 | `lessThan`
小于等于 | `lessThanOrEqualTo`
{% endblock %}

{% block code_query_lessThan %}

```php
$query->lessThan("priority", 2);
```
{% endblock %}

{% block code_query_greaterThanOrEqualTo %}

```php
$query->greaterThanOrEqualTo("priority", 2);
```
{% endblock %}

{% block code_query_with_regular_expression %}

```php
$query = new LeanQuery("Todo");
$query->matches("title","[\\u4e00-\\u9fa5]");
```
{% endblock %}

{% block code_query_with_contains_keyword %}

```php
$query = new LeanQuery("Todo");
$query->contains("title","李总");
```
{% endblock %}

{% block code_query_with_not_contains_keyword_using_regex %}

```php
$query = new LeanQuery("Todo");
$query->matches("title","^((?!机票).)*$");
```
{% endblock %}

{% block code_query_with_not_contains_keyword %}

```php
$query = new LeanQuery("Todo");
$query->notContainedIn("title",array("出差", "休假"));
$query->find();
```
{% endblock %}

{% block code_query_array_contains_using_equalsTo %}

```php
$query = new LeanQuery("Todo");
$date = new \DateTime("2015-11-11 08:30:00");
// equalTo: 可以找出数组中包含单个值的对象
$query->equalTo("reminders", $date);
$query->find();

```
{% endblock %}

{% block code_query_array_contains_all %}

```php
$query = new LeanQuery("Todo");
$date1 = new \DateTime("2015-11-11 08:30:00");
$date2 = new \DateTime("2015-11-11 09:30:00");
$query->containsAll("reminders", array($date1, $date2));
$query->find();
```
{% endblock %}

{% block code_query_whereHasPrefix %}

```php
// 找出开头是「早餐」的 Todo
$query = new LeanQuery("Todo");
$query->startsWith("content", "早餐");
```
{% endblock %}

{% block code_query_comment_by_todoFolder %}

```php
$query = new LeanQuery("Comment");
$query->equalTo("targetTodoFolder", LeanObject::create("TodoFolder", "5590cdfde4b00f7adb5860c8"));
```
{% endblock %}

{% block code_create_tag_object %}

```php
$tag = new LeanObject("Tag"); // 构建对象
$tag->set("name", "今日必做"); // 设置名称
$tag->save();
```
{% endblock %}

{% block code_create_family_with_tag %}

```php
$tag1 = new LeanObject("Tag"); // 构建对象
$tag1->set("name", "今日必做"); // 设置 Tag 名称

$tag2 = new LeanObject("Tag"); // 构建对象
$tag2->set("name", "老婆吩咐"); // 设置 Tag 名称

$tag3 = new LeanObject("Tag"); // 构建对象
$tag3->set("name", "十分重要"); // 设置 Tag 名称

$todoFolder = new LeanObject("TodoFolder"); // 构建对象
$todoFolder->set("name", "家庭"); // 设置 Todo 名称
$todoFolder->set("priority", 1); // 设置优先级

$relation = $todoFolder->getRelation("tags");
$relation->add($tag1);
$relation->add($tag2);
$relation->add($tag3);

$todoFolder->save(); // 保存到云端
```
{% endblock %}

{% block code_query_tag_for_todoFolder %}

```php
$todoFolder = LeanObject::create("TodoFolder", "5661047dddb299ad5f460166");
$relation   = $todoFolder->getRelation("tags");
$query      = $relation->getQuery();
// 结果将包含当前 todoFolder 的所有 Tag 对象
$query->find();
```
{% endblock %}

{% block code_query_todoFolder_with_tag %}

```php
$tag = LeanObject::create("Tag", "5661031a60b204d55d3b7b89");
$query = new LeanQuery("TodoFolder");
$query->equalTo("tags", $tag);
// 结果是所有包含当前 tag 的 TodoFolder 对象
$query->find();
```
{% endblock %}

{% block code_query_comment_include_todoFolder %}

```php
$commentQuery = new LeanQuery("Comment");
$commentQuery->descend("createdAt");
$commentQuery->limit(10);
$commentQuery->_include("targetTodoFolder"); // 关键代码，用 _include 告知服务端需要返回的关联属性对应的对象的详细信息，而不仅仅是 objectId
$comments = $commentQuery->find();
// 最近的十条评论, 其 targetTodoFolder 字段也有相应数据
forEach($comments as $comment) {
    $folder = $comment->get("targetTodoFolder");
}
```
{% endblock %}

{% block code_query_find_first_object %}

```php
$query = new LeanQuery("Todo");
$query->equalTo("priority",0);
// 返回符合条件的第一个对象
$query->first();
```
{% endblock %}

{% block code_set_query_limit %}

```php
$query = new LeanQuery("Todo");
$query->lessThanOrEqualTo("createdAt", new \DateTime());
$query->limit(10); // 最多返回 10 条结果
```
{% endblock %}

{% block code_set_skip_for_pager %}

```php
$query = new LeanQuery("Todo");
$query->lessThanOrEqualTo("createdAt", new \DateTime());
$query->limit(10); // 最多返回 10 条结果
$query->skip(20);  // 跳过 20 条结果
```

{% endblock %}

{% block code_query_count %}

```php
$query = new LeanQuery("Todo");
$query->equalTo("status", 0);
$query->count();
```

{% endblock %}

{% block code_query_comment_match_query_todoFolder %}

```php
// 构建内嵌查询
$innerQuery = new LeanQuery("TodoFolder");
$innerQuery->greaterThan("likes", 20);
// 将内嵌查询赋予目标查询
$query = new LeanQuery("Comment");
// 执行内嵌操作
$query->matchesInQuery("targetTodoFolder", $innerQuery);
// 返回符合 TodoFolder 超过 20 个赞这一条件的 Comment 对象集合
$comments = $query->find();

// 注意如果要做相反的查询可以使用
$query->notMatchInQuery("targetTodoFolder", $innerQuery);
// 如此做将查询出 likes 小于或者等于 20 的 TodoFolder 的 Comment 对象
$comments = $query->find();
```
{% endblock %}

{% block code_query_select_keys %}

```php
$query = new LeanQuery("Todo");
$query.select("title", "content");
$todos = $query->find();
forEach($todos as $todo) {
    $title   = $todo->get("title");
    $content = $todo->get("content");
    // 访问其它字段会返回 null
    $null = $todo->get("location");
}
```
{% endblock %}

{% block code_query_orderby %}
```php
// 按时间，升序排列
$query->ascend("createdAt");

// 按时间，降序排列
$query->descend("createdAt");
```
{% endblock %}

{% block code_query_orderby_on_multiple_keys %}

```php
$query->addAscend("priority");
$query->addDescend("createdAt");
```
{% endblock %}

{% block code_query_where_keys_exist %}

```php
// 存储一个带有图片的 Todo 到 LeanCloud 云端
$aTodoAttachmentImage = LeanFile::createWithUrl("test.jpg", "http://www.zgjm.org/uploads/allimg/150812/1_150812103912_1.jpg");
$todo = new LeanObject("Todo");
$todo->set("images", $aTodoAttachmentImage);
$todo->set("content", "记得买过年回家的火车票！！！");
$todo->save();

// 使用非空值查询获取有图片的 Todo
$query = new LeanQuery("Todo");
$query->exists("images");
// 返回有图片的 Todo 集合
$query->find();

// 使用空值查询获取没有图片的 Todo
$query->notExists("images");
```
{% endblock %}

{% block code_query_with_or %}

```php
$priorityQuery = new LeanQuery("Todo");
$priorityQuery->greaterThanOrEqualTo("priority", 3);

$statusQuery = new LeanQuery("Todo");
$statusQuery->equalTo("status", 1);

$query = LeanQuery::orQuery($priorityQuery, $statusQuery);

// 返回 priority 大于等于3 或 status 等于 1 的 Todo
$query->find();
```
{% endblock %}

{% block code_query_with_and %}

```php
$priorityQuery = new LeanQuery("Todo");
$priorityQuery->lessThan("priority", 3);

$statusQuery = new LeanQuery("Todo");
$statusQuery->equalTo("status", 0);

$query = LeanQuery::andQuery($priorityQuery, $statusQuery);

// 返回 priority 小于 3 并且 status 等于 0 的 Todo
$query->find();
```
{% endblock %}

{% block code_delete_todo_by_cql %}

```php
LeanQuery::doCloudQuery("delete from Todo where objectId='558e20cbe4b060308e3eb36c'");
```
{% endblock %}

{% block code_query_by_cql %}

```php

$todos = LeanQuery::doCloudQuery("select * from Todo where status = 1");

$result = LeanQuery::doCloudQuery("select count(*) from Todo where priority = 0");
$result["count"];
```
{% endblock %}

{% block code_query_by_cql_with_placeholder %}

```php
$todos = LeanQuery::doCloudQuery("select * from Todo where status = ? and priority = ?", array(0, 1));
```
{% endblock %}

{% block text_query_cache_intro %}{% endblock %}
{% block code_set_cache_policy %}{% endblock %}
{% block table_cache_policy %}{% endblock %}
{% block code_cache_operation %}{% endblock %}

{% block code_query_geoPoint_near %}

```php
$query = new LeanQuery("Todo");
$point = new GeoPoint(39.9, 116.4);
$query->limit(10);
$query->near("whereCreated", $point);
// 离这个位置最近的 10 个 Todo 对象
$nearbyTodos = $query->find();
```

在上面的代码中，`nearbyTodos` 返回的是与 `point` 这一点按距离排序（由近到远）的对象数组。注意：**如果在此之后又使用了 `ascend` 或 `descend` 方法，则按距离排序会被新排序覆盖。**
{% endblock %}

{% block text_platform_geoPoint_notice %}
{% endblock %}

{% block code_query_geoPoint_within %}

```php
$query->withinKilometers("whereCreated", $point, 2.0);
```
{% endblock %}

{% block link_to_relation_guide_doc %}(**PHP 文档待补充**){% endblock %}

{% block code_send_sms_code_for_loginOrSignup %}

```php
LeanCloud::requestLoginSmsCode("13577778888");
```
{% endblock %}

{% block code_verify_sms_code_for_loginOrSignup %}

```php
LeanCloud::logInWithSmsCode("13577778888", "123456");
```
{% endblock %}

{% block code_user_signUp_with_username_and_password %}

```php
$user = new LeanUser();              // 新建 User 对象实例
$user->setUsername("Tom");           // 设置用户名
$user->setPassword("cat!@#123");     // 设置密码
$user->setEmail("tom@leancloud.cn"); // 设置邮箱
$user->signUp();
```
{% endblock %}

{# 2016-06-28 请不要删除 text_using_async_methods，并保持空白。 #}
{% block text_using_async_methods %}{% endblock %}

{% block code_user_logIn_with_username_and_password %}

```php
LeanUser::logIn("Tom", "cat!@#123");
```
{% endblock %}

{% block code_user_logIn_with_mobilephonenumber_and_password %}

```php
LeanUser::logInWithMobilePhoneNumber("13577778888", "cat!@#123");
```
{% endblock %}

{% block code_user_logIn_requestLoginSmsCode %}

```php
LeanUser::requestLoginSmsCode("13577778888");
```
{% endblock %}

{% block code_user_logIn_with_smsCode %}

```php
LeanUser::logInWithSmsCode("13577778888", "238825");
```
{% endblock %}

{% block code_get_user_properties %}

```php
$currentUser = LeanUser::getCurrentUser();
$currentUser->getUsername();
$currentUser->getEmail();
// 请注意，以下代码无法获取密码

$currentUser->getPassword(); // 无 getPassword() 方法
```
{% endblock %}

{% block code_set_user_custom_properties %}

```php
$currentUser = LeanUser::getCurrentUser();
$currentUser->set("age", 25);
$currentUser->save();
```
{% endblock %}

{% block code_update_user_custom_properties %}

```php
$currentUser->set("age", 27);
$currentUser->save();
```
{% endblock %}

{% block code_reset_password_by_email %}

```php
LeanUser::requestPasswordReset("myemail@example.com");
```
{% endblock %}

{% block code_reset_password_by_mobilephoneNumber %}

```php
LeanUser::requestPasswordResetBySmsCode("18612340000");
```
{% endblock %}

{% block code_reset_password_by_mobilephoneNumber_verify %}

```php
LeanUser::resetPasswordBySmsCode("123456", "password");
```
{% endblock %}

{% block code_current_user %}

```php
$currentUser = LeanUser::getCurrentUser();

if ($currentUser != null) {
    // 跳转到首页
} else {
    //缓存用户对象为空时，可打开用户注册界面…
}
```
{% endblock %}

{% block code_current_user_logout %}

```php
LeanUser->logOut(); // 退出当前用户
$currentUser = LeanUser::getCurrentUser(); // 现在的 currentUser 是 null 了
```
{% endblock %}

{% block code_query_user %}

```php
$userQuery = new LeanQuery("_User");
```
{% endblock %}

{% block text_subclass %}
## 子类化
LeanCloud 希望设计成能让人尽快上手并使用。你可以通过 `LeanObject#get` 方法访问所有的数据。但是在很多现有成熟的代码中，子类化能带来更多优点，诸如简洁、可扩展性以及 IDE 提供的代码自动完成的支持等等。子类化不是必须的，你可以将下列代码转化：

```
$student = new LeanObject("Student");
$student->set("name", "小明");
$student->save();
```

可改写成:

```
$student = new Student();
$student->set("name", "小明");
$student->save();
```

这样代码看起来是不是更简洁呢？

### 子类化 AVObject

要实现子类化，需要下面几个步骤：

1. 首先声明一个子类继承自 `LeanObject`；
2. 子类中声明静态字段 `protected static $className`，对应云端的数据表名；
3. 建议不要重载构造函数 `__construct()`，如果一定需要构造，请确保其接受 2 个参数 `$className` 和 `$objectId`；
4. 将子类注册到 `LeanObject`，如 `Student::registerClass();`。

下面是实现 `Student` 子类化的例子:

```php
// Student.php
use LeanCloud\LeanObject;

class Student extends LeanObject {
    protected static $className = "Student";
}
Student::registerClass();
```

### 访问器、修改器和方法

添加方法到 LeanObject 的子类有助于封装类的逻辑。你可以将所有跟子类有关的逻辑放到一个地方，而不是分成多个类来分别处理商业逻辑和存储/转换逻辑。

你可以很容易地添加访问器和修改器到你的 LeanObject 子类。像平常那样声明字段的`getter` 和 `setter` 方法，但是通过 LeanObject 的 `get` 和 `set` 方法来实现它们。下面是这个例子为 `Student` 类创建了一个 `content` 的字段：

```php
// Student.php
use LeanCloud\LeanObject;

class Student extends LeanObject {
    protected static $className = "Student";

    public function setContent($value) {
        $this->set("content", $value);
        return $this; // 方便链式调用
    }

    public function getContent() {
        return $this->get("content");
    }
}
Student::registerClass();
```

现在你就可以使用 `$student->getContent()` 方法来访问 `content` 字段，
并通过 `$student->setContent("blah blah blah")` 来修改它。

各种数据类型的访问器和修改器都可以这样被定义，使用各种 `get()` 方法的
变种，例如 `getInt()`，`getLeanFile()` 或者 `getMap()`。

如果你不仅需要一个简单的访问器，而是有更复杂的逻辑，你也可以实现自己的方法，例如：

```php
public function takeAccusation() {
  // 处理用户举报，当达到某个条数的时候，自动打上屏蔽标志
  $this->increment("accusation", 1);
  if ($this->getAccusation() > 50) {
    $this->setSpam(true);
  }
}
```

### 初始化子类

你可以使用你自定义的构造函数来创建你的子类对象。`LeanObject` 已定义了默认的构造函数，如果需要重载构造函数，请注意其需要接收 2 个参数：`$className` 和 `$objectId`。这个构造函数将会被 SDK 使用来创建子类的对象。

要创建一个到现有对象的引用，可以使用 `LeanObject::create("Student", "abc123")`:

```php
$student = LeanObject::create("Student", "573a8459df0eea005e6b711c");
```

### 查询子类

你可以通过 `LeanObject#getQuery()` 方法获取特定的子类的查询对象。下面的例子就查询了用户发表的所有微博列表：

```php
$query = $post->getQuery();
$query->equalTo("pubUser", LeanUser::getCurrentUser()->getUsername());
$query->find();
```
{% endblock %}

{% block link_to_in_app_search_doc %}（**PHP 不支持**）{% endblock %}
{% block link_to_status_system_doc %}（**PHP 不支持**）{% endblock %}
{% block link_to_sns_doc %}（**PHP 文档待补充**）{% endblock %}
{% block link_to_feedback_doc %}（**PHP 不支持**）{% endblock %}


{# 2016-06-07 以下三部分都不适用于 Python，所以清空内容。 #}
{% block text_work_in_background %}{% endblock %}
{% block text_data_protocol %}{% endblock %}
{% block save_eventually %}{% endblock %}

{# --End--主模板留空的代码段落，子模板根据自身实际功能给予实现 #}
