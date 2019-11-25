![CLEVER DATA GIT REPO](https://raw.githubusercontent.com/LiCongMingDeShujuku/git-resources/master/0-clever-data-github.png "李聪明的数据库")

# POST TITLE
#### SECONDARY TITLE
**TIME STAMP**

![#](images/##############?raw=true "#")

## Contents

- [中文](#中文)
- [English](#English)
- [SQL Logic](#Logic)
- [Build Info](#Build-Info)
- [Author](#Author)
- [License](#License) 


## 中文
这是我编写的一些用于重建所有表中的所有索引的SQL逻辑。还会包含一些额外的代码来识别那些碎片超过70％的索引为REBUILD，而那些低于70％的索引则为REORGANIZE。此脚本未设置为重建并使表保持在线状态。为此你需要添加* WITH（ONLINE = ON）。因此，你应该将其作为一般维护。如果不使用ONLINE = ON，直接重建速度要快得多。我并不反对在网上这样做，但我确实主张采用最快的方法。此过程将针对从information_schema.tables反映为BASE TABLE的表。另外，它不会以系统数据库为目标，因此Master，Model，Msdb和Tempdb都被排除在外。


你会发现一些替代品正在进行中。这样，负责生成rebuild语句的逻辑将创建适当的变量，将数据库名称添加为后缀以确保唯一性。很明显得是如果添加到变量名称的数据库名称有任何空格，点(.)等，它将打破逻辑流程，因此我只需用下划线替换空格，并用下划线替换点(.)。很简单。


运行以下逻辑：


## English
Here’s some SQL logic I wrote up to rebuild ALL the indexes across ALL tables. Later I may include some extra code to identify those indexes that are fragmented above 70% to be REBUILD, and those indexes that are below 70% to be REORGANIZE. This script is not set to rebuild and leave tables online. For this you need to add the *WITH (ONLINE = ON); therefore you should run this as general maintenance after hours. It’s just much faster to rebuild directly without using the ONLINE = ON. I’m not against doing it online, but I do advocate for the quickest approach normally. This process will target tables that are reflected as BASE TABLE from information_schema.tables. Additionally; it will not target the system databases so Master, Model, Msdb, and Tempdb are all excluded.


You will notice some replacements going on. This is so the logic responsible for producing the rebuild statement will create the appropriate variables adding the database name as the suffix to ensure uniqueness. Obviously; if the database name that it’s adding to the variable name has any spaces, dots(.), etc. it will break the logical flow so to address this I simply replace the spaces with underscores, and the dots(.) also with underscores. Easy.

On with the logic.

---
## Logic
```SQL
use master;
set nocount on
 
declare @index_maintenance  varchar(max)
set @index_maintenance  = ''
select  @index_maintenance  = @index_maintenance +
'use [' + sd.name + '];' + char(10) +
'declare    @rebuild_indexes_in_' + replace(replace(sd.name, ' ', '_'), '.', '_') + '   varchar(max)'   + char(10) +
'set        @rebuild_indexes_in_' + replace(replace(sd.name, ' ', '_'), '.', '_') + '   = '''''         + char(10) +
'select @rebuild_indexes_in_' + replace(replace(sd.name, ' ', '_'), '.', '_') + '   = @rebuild_indexes_in_' + replace(replace(sd.name, ' ', '_'), '.', '_') + ' +
''alter index all on [dbo].['' + object_name(sddips.object_id) + ''] rebuild;'' + char(10)
from sys.dm_db_index_physical_stats (db_id(), null, null, null, null) sddips
join information_schema.TABLES ist on object_name(sddips.object_id) = ist.table_name
where ist.table_type = ''base table''
exec (@rebuild_indexes_in_' + replace(replace(sd.name, ' ', '_'), '.', '_') + ')' + CHAR(10) + CHAR(10)
from sys.databases sd where name not in ('master', 'model', 'msdb', 'tempdb') order by name asc
select  (@index_maintenance) for xml path(''), type

```

[![WorksEveryTime](https://forthebadge.com/images/badges/60-percent-of-the-time-works-every-time.svg)](https://shitday.de/)

## Build-Info

| Build Quality | Build History |
|--|--|
|<table><tr><td>[![Build-Status](https://ci.appveyor.com/api/projects/status/pjxh5g91jpbh7t84?svg?style=flat-square)](#)</td></tr><tr><td>[![Coverage](https://coveralls.io/repos/github/tygerbytes/ResourceFitness/badge.svg?style=flat-square)](#)</td></tr><tr><td>[![Nuget](https://img.shields.io/nuget/v/TW.Resfit.Core.svg?style=flat-square)](#)</td></tr></table>|<table><tr><td>[![Build history](https://buildstats.info/appveyor/chart/tygerbytes/resourcefitness)](#)</td></tr></table>|

## Author

- **李聪明的数据库 Lee's Clever Data**
- **Mike的数据库宝典 Mikes Database Collection**
- **李聪明的数据库** "Lee Songming"

[![Gist](https://img.shields.io/badge/Gist-李聪明的数据库-<COLOR>.svg)](https://gist.github.com/congmingshuju)
[![Twitter](https://img.shields.io/badge/Twitter-mike的数据库宝典-<COLOR>.svg)](https://twitter.com/mikesdatawork?lang=en)
[![Wordpress](https://img.shields.io/badge/Wordpress-mike的数据库宝典-<COLOR>.svg)](https://mikesdatawork.wordpress.com/)

---
## License
[![LicenseCCSA](https://img.shields.io/badge/License-CreativeCommonsSA-<COLOR>.svg)](https://creativecommons.org/share-your-work/licensing-types-examples/)

![Lee Songming](https://raw.githubusercontent.com/LiCongMingDeShujuku/git-resources/master/1-clever-data-github.png "李聪明的数据库")

