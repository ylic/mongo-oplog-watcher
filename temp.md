参考链接

http://landcareweb.com/questions/1519/ru-he-jian-ting-mongodbji-he-de-bian-hua

https://jwage.com/posts/2011/03/16/mongodb-tailable-cursors/


您可以使用启动服务器 --replSet 选项，它将创建/填充 oplog。即使没有中学。这绝对是“监听”DB中更改的唯一方法。 - Gates VP


MongoDB有所谓的 capped collections 和 tailable cursors 允许MongoDB将数据推送到侦听器。

一个 capped collection 本质上是一个固定大小的集合，只允许插入。以下是创建一个的样子：

db.createCollection("messages", { capped: true, size: 100000000 })
MongoDB Tailable游标（Jonathan H. Wage的原帖）
红宝石

<code>
coll = db.collection('my_collection')
cursor = Mongo::Cursor.new(coll, :tailable => true)
loop do
  if doc = cursor.next_document
    puts doc
  else
    sleep 1
  end
end
 </code>
  
  
  
PHP
<code>
$mongo = new Mongo();
$db = $mongo->selectDB('my_db')
$coll = $db->selectCollection('my_collection');
$cursor = $coll->find()->tailable(true);
while (true) {
    if ($cursor->hasNext()) {
        $doc = $cursor->getNext();
        print_r($doc);
    } else {
        sleep(1);
    }
}
</code>
蟒蛇 （通过 罗伯特斯图尔特）

from pymongo import Connection
import time

db = Connection().my_db
coll = db.my_collection
cursor = coll.find(tailable=True)
while cursor.alive:
    try:
        doc = cursor.next()
        print doc
    except StopIteration:
        time.sleep(1)
Perl的 （通过 马克斯）

use 5.010;

use strict;
use warnings;
use MongoDB;

my $db = MongoDB::Connection->new;
my $coll = $db->my_db->my_collection;
my $cursor = $coll->find->tailable(1);
for (;;)
{
    if (defined(my $doc = $cursor->next))
    {
        say $doc;
    }
    else
    {
        sleep 1;
    }
}


