Spring Data Jpa注解
Entity篇
@Entity
定义对象将会成为被JPA管理的实体,将映射到指定的数据库表。
@Table
@Table(name="users")
数据库表名
@Id
定义属性为数据库的主键

@GeneratedValue
ID生成策略为自动生成--
通过strategy 属性指明主键生成策略默认是AUTO
1. IDENTITY：采用数据库ID自增长， Oracle 不支持这种方式；
2. AUTO：JPA 自动选择合适的策略，是默认选项；
3. SEQUENCE：通过序列产生主键，通过 @SequenceGenerator 注解指定序列名， MySql 不支持这种方式；
4. TABLE：通过表产生主键，框架借由表模拟序列产生主键，使用该策略可以使应用更易于数据库移植。
@Basic
@Basic表示属性到数据库表的字段的映射。如果域上没有人和注解，默认即为@Basic。
@Basic参数:
    1.fetch（default:EAGER）
        1.1 EAGER   主动抓取
        1.2 LAZY    延迟加载
    2.optional (default:true)
        表示该属性是否允许为
@Column
定义该属性是数据库中的一列
1.name(defautl:属性名称): 指定映射列名
2.unique(default:false): 是否唯一
3.nullable(default:true): 是否允许为null
4.length: 指定列最大字符长度
5.insertable(default:true): 是否允许插入
6.updatetable(default:true): 是否允许更新，对于创建后不允许更新的字段应该使用该参数
7.columnDefinition: 表示该字段在数据库中的实际类型
@Transient
表示该属性并非一个到数据库表的字段的映射，JPA将忽略该属性。
@Temporal
@Temporal用来设置Date类型的属性映射到对应精度的字段。
    1.@Temporal(TemporalType.DATE)映射为日期 // date （只有日期）
    2.@Temporal(TemporalType.TIME)映射为日期 // time （是有时间）
    3.@Temporal(TemporalType.TIMESTAMP)映射为日期 // date time （日期+时间）
@Lob
将属性映射成数据库支持的大对象类型。
    1. Clob（Character Large Ojects）类型是长字符串类型，java.sql.Clob、Character[]、char[] 和 String 将被映射为 Clob 类型。
    2. Blob（Binary Large Objects）类型是字节类型，java.sql.Blob、Byte[]、byte[] 和 实现了Serializable接口的类型将被映射为 Blob 类型。
由于Clob，Blob占用内存空间较大一般配合@Basic(fetch=FetchType.LAZY)将其设置为延迟加载
@OneToOne
1. targetEntity 默认关联实体类型,默认当前注解的实体类。
2. cascade 级联操作策略
    2.1 CascadeType.PERSIST 级联新建
    2.2 CascadeType.REMOVE 级联删除
    2.3 CascadeType.REFRESH 级联刷新
    2.4 CascadeType.MERGE 级联更新
    2.5 CascadeType.ALL 四项全选
    2.6 不定义,关系表不会产生人和影响
3. fetch（default:EAGER）加载方式
    3.1 EAGER   主动抓取
    3.2 LAZY    延迟加载
4. optional (default:true) 关联属性是否为空
    如果为false，需要配合使用@JoinColumn注解。
5. mappedBy 表明当前类是关系的被维护方,另一方是关系维护方
    5.1 关系维护方 对应定义外键约束的数据库表
    5.2 关系被维护方 对应外键所在的数据库表
    注意：只有关系维护方才能操作两者的关系。被维护方即使设置了维护方属性进行存储也不会更新外键关联。
 //双向OneToOne
 @Entity
 public class BlogMetaInfo{
    @OneToOne(cascade = CASCADETYPE.ALL)
    @JoinColumn(unique=true)
    private User user;
    //geter seter方法省略
 }
 @Entity
 public class User{
    @OneToOne(cascade=CASCADETYPE.ALL,mappedBy="user")
    private BlogMetaInfo blogMetaInfo;
 }
 //User是关系被维护者，执行userRepository.save(user)方法时会向User和BlogMetaInfo表中插入数据，但是不会设置两者之间的关系，相当于BlogMetaInfo表中的user_id字段为空。
 //BlogMetaInfo是关系维护方，当执行blogMetaInfoRepository.save(blogMetaInfo)方法时两个表都会插入数据，user_id也会得到正确的设置。
@JoinColumn
定义外键关联的字段名称。
@ManyToOne
数据库中多对一（反过来是一对多），可以通过外键关联建立，可以单独定义数据表来保存关系。
@ManyToOne定义了多对一关系，如果生成DDL会默认生成Comment表到Blog表的一个外键关联。
@JoinColumn可以与之一同使用，设置外键关联的字段名称和约束。
1. targetEntity 默认关联实体类型,默认当前注解的实体类。
2. cascade 级联操作策略
    2.1 CascadeType.PERSIST 级联新建
    2.2 CascadeType.REMOVE 级联删除
    2.3 CascadeType.REFRESH 级联刷新
    2.4 CascadeType.MERGE 级联更新
    2.5 CascadeType.ALL 四项全选
    2.6 不定义,关系表不会产生人和影响
3. fetch（default:EAGER）加载方式
    3.1 EAGER   主动抓取
    3.2 LAZY    延迟加载
4. optional (default:true) 关联属性是否为空
    如果为false，需要配合使用@JoinColumn注解。
5. mappedBy 表明当前类是关系的被维护方,另一方是关系维护方
    5.1 关系维护方 对应定义外键约束的数据库表
    5.2 关系被维护方 对应外键所在的数据库表
    注意：只有关系维护方才能操作两者的关系。被维护方即使设置了维护方属性进行存储也不会更新外键关联。
//单项多对一 
@Entity
class Comment{
    @ManyToOne
    private Blog blog;
}
@Entity
class Blog{
}
//会默认生成Comment表到Blog表的一个外键关联。
//双向一对多 多个评论对应一个博客， 一个博客对应多个评论
@Entity
class Comment{
    @ManyToOne
    private Blog blog;
}
@Entity
class Blog{
    @OneToMany(mappedBy="blog")
    private List<Comment> comments;
}
@ManyToMany
一个博客可以拥有多个标签，一个标签也可以使用在多个博客上，Blog和Tag就是多对多关系。
@ManyToMany表示多对多，和@OneToOne、@ManyToOne一样也有单向双向之分。单项双向和注解没有关系，只看实体类之间是否相互引用。
//单向多对多
@Entity
public class Blog{
    @ManyToMany(cascade=CascadeType.ALL)
    private List<Tag> tags = new ArrayList<Tag>();
}
@Entity
public class Tag{
}
//双向多对多
@Entity
public class Blog{
    @ManyToMany(cascade=CascadeType.ALL)
    private List<Tag> tags = new ArrayList<Tag>();
}
@Entity
public class Tag{
    @ManyToMany(mappedBy="tags")
    private List<Blog> blogs = new ArrayList<Blog>();
}
@JoinTable
多对多关系如果需要自动生成DDL，则会生成一张描述多对多关系的表。这个例子中表名为blog_tag，表中有两个属性blog_id和tag_id。
如果需要定制表明和属性或者实际命名不相同时，则需要@JoinTable注解。
@Entity
public class Blog{
    @ManyToMany
    @JoinTable(
        name="blog_tag_relation",
        joinColumns=@JoinColumn(name="blog_id",referencedColumnName="id"),
        inverseJoinColumn=@JoinColumn(name="tag_id",referencedColumnName="id")
        private List<Tag> tags = new ArrayList<Tag>();
    )
}