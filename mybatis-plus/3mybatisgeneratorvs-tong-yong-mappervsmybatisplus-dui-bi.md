# 3.Mybatis-generatorVS通用MapperVSMybatis-Plus对比

|  | Mybatis-generator | 通用Mapper | Mybatis-Plus |
| :---: | :---: | :---: | :---: |
| 代码生成器 | 支持自动生成Model,Mapper,Mapper XML文件生成方式不够灵活；生成代码功能较为简单 | 支持自动生成Entity,Mapper,Mapper XML文件；提供通用的Mapper模板，生成方式较灵活；生成的Model文件包含注释能够很好地与数据库表完成映射 | 支持自动生成Entity,Mapper,Mapper XML,Service,Controller文件；提供BaseMapper接口 |
| CRUD操作 | 代码生成后每个Mapper有固定的CRUD方法；在每个Mapper上分别扩展 | 提供通用Mapper接口；可以扩展通用接口 | 提供BaseMapper接口；可以扩展通用接口 |
| 条件构造器 | 每个实体类自己的Example构造条件 | 提供通用Example | 提供Wrapper进行复杂条件构造 |
| 乐观锁 |  | 支持 | 支持 |
| 主键策略 |  |  | 支持 |
| 逻辑删除 |  |  | 支持 |
| 通用枚举 |  |  | 支持 |
| 攻击Sql阻断 |  |  | 支持 |
| 性能分析 |  |  | 支持 |

总结一下，通用Mapper是对Mybatis-generator的升级改造，解决了使用Mybatis-generator可能需要大量重构的问题，并且在这个基础上加入了一些新的功能。Mybatis-Plus可以看作是在另一个方向上对Mybatis的升级改造，不仅能够根据数据库表快速生成pojo实体类，还封装了大量CRUD方法，使用Wrapper解决了复杂条件构造等问题，更是根据开发中常见的问题给出了一系列解决方案。

在拥有Maven和Spring boot的开发框架下，MBG、通用Mapper和MP都可以快速地完成安装，相比于MBG和通用Mapper仅需要执行插件就可以完成基本的开发工作，MP可能需要更多的开发工作量。

