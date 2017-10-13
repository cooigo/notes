Newtonsoft.Json高级用法

基本用法

Json.Net是支持序列化和反序列化DataTable,DataSet,Entity Framework和Entity的。下面分别举例说明序列化和反序列化。

DataTable：

```csharp
            //序列化DataTable
            DataTable dt = new DataTable();
            dt.Columns.Add("Age", Type.GetType("System.Int32"));
            dt.Columns.Add("Name", Type.GetType("System.String"));
            dt.Columns.Add("Sex", Type.GetType("System.String"));
            dt.Columns.Add("IsMarry", Type.GetType("System.Boolean"));
            for (int i = 0; i < 4; i++)
            {
                DataRow dr = dt.NewRow();
                dr["Age"] = i + 1;
                dr["Name"] = "Name" + i;
                dr["Sex"] = i % 2 == 0 ? "男" : "女";
                dr["IsMarry"] = i % 2 > 0 ? true : false;
                dt.Rows.Add(dr);
            }
            Console.WriteLine(JsonConvert.SerializeObject(dt));
```


利用上面字符串进行反序列化
```csharp
 string json = JsonConvert.SerializeObject(dt);
 dt=JsonConvert.DeserializeObject<DataTable>(json);
 foreach (DataRow dr in dt.Rows)
 {
 　　Console.WriteLine("{0}\t{1}\t{2}\t{3}\t", dr[0], dr[1], dr[2], dr[3]);
 }
```

Entity序列化和DataTable一样，就不过多介绍了。


高级用法

1. 忽略某些属性

2. 默认值的处理

3. 空值的处理

4. 支持非公共成员

5. 日期处理

6. 自定义序列化的字段名称

7. 动态决定属性是否序列化

8. 枚举值的自定义格式化问题

9. 自定义类型转换

10. 全局序列化设置

一. 忽略某些属性

类似本问开头介绍的接口优化，实体中有些属性不需要序列化返回，可以使用该特性。首先介绍Json.Net序列化的模式:OptOut 和 OptIn

OptOut	默认值,类中所有公有成员会被序列化,如果不想被序列化,可以用特性JsonIgnore
OptIn	默认情况下,所有的成员不会被序列化,类中的成员只有标有特性JsonProperty的才会被序列化,当类的成员很多,但客户端仅仅需要一部分数据时,很有用
 

 

* 仅需要姓名属性

```csharp
    [JsonObject(MemberSerialization.OptIn)]
    public class Person
    {
        public int Age { get; set; }

        [JsonProperty]
        public string Name { get; set; }

        public string Sex { get; set; }

        public bool IsMarry { get; set; }

        public DateTime Birthday { get; set; }
    }
```


* 不需要是否结婚属性

```csharp
    [JsonObject(MemberSerialization.OptOut)]
    public class Person
    {
        public int Age { get; set; }

        public string Name { get; set; }

        public string Sex { get; set; }

        [JsonIgnore]
        public bool IsMarry { get; set; }

        public DateTime Birthday { get; set; }
    }
```


  通过上面的例子可以看到，要实现不返回某些属性的需求很简单。

  1. 在实体类上加上[JsonObject(MemberSerialization.OptOut)] 

  2. 在不需要返回的属性上加上 [JsonIgnore]说明。

二. 默认值处理

序列化时想忽略默认值属性可以通过JsonSerializerSettings.DefaultValueHandling来确定，该值为枚举值

DefaultValueHandling.Ignore

序列化和反序列化时,忽略默认值

DefaultValueHandling.Include

序列化和反序列化时,包含默认值
  
```csharp
 [DefaultValue(10)]
 public int Age { get; set; }
 Person p = new Person { Age = 10, Name = "张三丰", Sex = "男", IsMarry = false, 
 Birthday = new DateTime(1991, 1, 2) };
 JsonSerializerSettings jsetting=new JsonSerializerSettings();
 jsetting.DefaultValueHandling=DefaultValueHandling.Ignore;
 Console.WriteLine(JsonConvert.SerializeObject(p, Formatting.Indented, jsetting));
 ```
最终结果如下：
 

三.空值的处理

　　序列化时需要忽略值为NULL的属性，可以通过JsonSerializerSettings.NullValueHandling来确定，另外通过JsonSerializerSettings设置属性是对序列化过程中所有属性生效的，想单独对某一个属性生效可以使用JsonProperty，下面将分别展示两个方式

　　1.JsonSerializerSettings
```csharp
 Person p = new Person { room=null,Age = 10, Name = "张三丰", Sex = "男", IsMarry = false, Birthday = new DateTime(1991, 1, 2) };
 JsonSerializerSettings jsetting=new JsonSerializerSettings();
 jsetting.NullValueHandling = NullValueHandling.Ignore;
 Console.WriteLine(JsonConvert.SerializeObject(p, Formatting.Indented, jsetting));
```

   2.JsonProperty



通过JsonProperty属性设置的方法，可以实现某一属性特别处理的需求，如默认值处理，空值处理，自定义属性名处理，格式化处理。上面空值处理实现

```csharp
 [JsonProperty(NullValueHandling=NullValueHandling.Ignore)]
 public Room room { get; set; }
```

四.支持非公共成员

  序列化时默认都是处理公共成员，如果需要处理非公共成员，就要在该成员上加特性"JsonProperty"

 [JsonProperty]
 private int Height { get; set; }
 

五.日期处理

　　对于Dateime类型日期的格式化就比较麻烦了，系统自带的会格式化成iso日期标准，但是实际使用过程中大多数使用的可能是yyyy-MM-dd 或者yyyy-MM-dd HH:mm:ss两种格式的日期，解决办法是可以将DateTime类型改成string类型自己格式化好，然后在序列化。如果不想修改代码，可以采用下面方案实现。

Json.Net提供了IsoDateTimeConverter日期转换这个类，可以通过JsnConverter实现相应的日期转换

```csharp
[JsonConverter(typeof(IsoDateTimeConverter))]
public DateTime Birthday { get; set; }
```

但是IsoDateTimeConverter日期格式不是我们想要的，我们可以继承该类实现自己的日期

```csharp
public class ChinaDateTimeConverter : DateTimeConverterBase
{
    private static IsoDateTimeConverter dtConverter = new IsoDateTimeConverter { DateTimeFormat = "yyyy-MM-dd" };

    public override object ReadJson(JsonReader reader, Type objectType, object existingValue, JsonSerializer serializer)
    {
        return dtConverter.ReadJson(reader, objectType, existingValue, serializer);
    }

    public override void WriteJson(JsonWriter writer, object value, JsonSerializer serializer)
    {
        dtConverter.WriteJson(writer, value, serializer);
    }
}
```

自己实现了一个yyyy-MM-dd格式化转换类，可以看到只是初始化IsoDateTimeConverter时给的日期格式为yyyy-MM-dd即可，下面看下效果

```csharp
[JsonConverter(typeof(ChinaDateTimeConverter))]
public DateTime Birthday { get; set; }
```

可以根据自己需求实现不同的转换类

六.自定义序列化的字段名称

实体中定义的属性名可能不是自己想要的名称，但是又不能更改实体定义，这个时候可以自定义序列化字段名称。

```csharp
[JsonProperty(PropertyName = "CName")]
public string Name { get; set; }
```

七.动态决定属性是否序列化

这个是为了实现@米粒儿提的需求特别增加的，根据某些场景，可能A场景输出A，B，C三个属性，B场景输出E，F属性。虽然实际中不一定存在这种需求，但是json.net依然可以支持该特性。继承默认的DefaultContractResolver类，传入需要输出的属性重写修改了一下，大多数情况下应该是要排除的字段少于要保留的字段,  为了方便书写这里修改了构造函数加入retain表示props是需要保留的字段还是要排除的字段

```csharp
public class LimitPropsContractResolver : DefaultContractResolver
{
    string[] props = null;

    bool retain;

    /// <summary>
    /// 构造函数
    /// </summary>
    /// <param name="props">传入的属性数组</param>
    /// <param name="retain">true:表示props是需要保留的字段  false：表示props是要排除的字段</param>
    public LimitPropsContractResolver(string[] props, bool retain=true)
    {
        //指定要序列化属性的清单
        this.props = props;

        this.retain = retain;
    }

    protected override IList<JsonProperty> CreateProperties(Type type,

    MemberSerialization memberSerialization)
    {
        IList<JsonProperty> list =
        base.CreateProperties(type, memberSerialization);
        //只保留清单有列出的属性
        return list.Where(p => {
            if (retain)
            {
                return props.Contains(p.PropertyName);
            }
            else
            {
                return !props.Contains(p.PropertyName);
            }      
        }).ToList();
    }
}
```

```csharp
public int Age { get; set; }

[JsonIgnore]
public bool IsMarry { get; set; }

public string Sex { get; set; }
```

```csharp
JsonSerializerSettings jsetting=new JsonSerializerSettings();
jsetting.ContractResolver = new LimitPropsContractResolver(new string[] { "Age", "IsMarry" });
Console.WriteLine(JsonConvert.SerializeObject(p, Formatting.Indented, jsetting));

```
  使用自定义的解析类，只输出"Age", "IsMarry"两个属性，看下最终结果.只输出了Age属性，为什么IsMarry属性没有输出呢，因为标注了JsonIgnore

 

 看到上面的结果想要实现pc端序列化一部分，手机端序列化另一部分就很简单了吧，我们改下代码实现一下

```csharp
  string[] propNames = null;
  if (p.Age > 10)
  {
  　　propNames = new string[] { "Age", "IsMarry" };
  }
  else
  {
      propNames = new string[] { "Age", "Sex" };
  }
  jsetting.ContractResolver = new LimitPropsContractResolver(propNames);
  Console.WriteLine(JsonConvert.SerializeObject(p, Formatting.Indented, jsetting));
```
 

八.枚举值的自定义格式化问题

  默认情况下对于实体里面的枚举类型系统是格式化成改枚举对应的整型数值,那如果需要格式化成枚举对应的字符怎么处理呢？Newtonsoft.Json也帮我们想到了这点，下面看实例

```csharp
public enum NotifyType
{
    /// <summary>
    /// Emil发送
    /// </summary>
    Mail=0,

    /// <summary>
    /// 短信发送
    /// </summary>
    SMS=1
}

public class TestEnmu
{
    /// <summary>
    /// 消息发送类型
    /// </summary>
    public NotifyType Type { get; set; }
}
JsonConvert.SerializeObject(new TestEnmu());
```
输出结果：  现在改造一下，输出"Type":"Mail"

```csharp
public class TestEnmu
{
    /// <summary>
    /// 消息发送类型
    /// </summary>
    [JsonConverter(typeof(StringEnumConverter))]
    public NotifyType Type { get; set; }
}
```
其它的都不变，在Type属性上加上了JsonConverter(typeof(StringEnumConverter))表示将枚举值转换成对应的字符串,而StringEnumConverter是Newtonsoft.Json内置的转换类型,最终输出结果

九.自定义类型转换

默认情况下对于实体里面的Boolean系统是格式化成true或者false,对于true转成"是" false转成"否"这种需求改怎么实现了？我们可以自定义类型转换实现该需求，下面看实例

```csharp
public class BoolConvert : JsonConverter
{
    private string[] arrBString { get; set; }

    public BoolConvert()
    {
        arrBString = "是,否".Split(',');
    }

    /// <summary>
    /// 构造函数
    /// </summary>
    /// <param name="BooleanString">将bool值转换成的字符串值</param>
    public BoolConvert(string BooleanString)
    {
        if (string.IsNullOrEmpty(BooleanString))
        {
            throw new ArgumentNullException();
        }
        arrBString = BooleanString.Split(',');
        if (arrBString.Length != 2)
        {
            throw new ArgumentException("BooleanString格式不符合规定");
        }
    }


    public override object ReadJson(JsonReader reader, Type objectType, object existingValue, JsonSerializer serializer)
    {
        bool isNullable = IsNullableType(objectType);
        Type t = isNullable ? Nullable.GetUnderlyingType(objectType) : objectType;

        if (reader.TokenType == JsonToken.Null)
        {
            if (!IsNullableType(objectType))
            {
                throw new Exception(string.Format("不能转换null value to {0}.", objectType));
            }

            return null;
        }

        try
        {
            if (reader.TokenType == JsonToken.String)
            {
                string boolText = reader.Value.ToString();
                if (boolText.Equals(arrBString[0], StringComparison.OrdinalIgnoreCase))
                {
                    return true;
                }
                else if (boolText.Equals(arrBString[1], StringComparison.OrdinalIgnoreCase))
                {
                    return false;
                }
            }

            if (reader.TokenType == JsonToken.Integer)
            {
                //数值
                return Convert.ToInt32(reader.Value) == 1;
            }
        }
        catch (Exception ex)
        {
            throw new Exception(string.Format("Error converting value {0} to type '{1}'", reader.Value, objectType));
        }
        throw new Exception(string.Format("Unexpected token {0} when parsing enum", reader.TokenType));
    }

    /// <summary>
    /// 判断是否为Bool类型
    /// </summary>
    /// <param name="objectType">类型</param>
    /// <returns>为bool类型则可以进行转换</returns>
    public override bool CanConvert(Type objectType)
    {
        return true;
    }


    public bool IsNullableType(Type t)
    {
        if (t == null)
        {
            throw new ArgumentNullException("t");
        }
        return (t.BaseType.FullName=="System.ValueType" && t.GetGenericTypeDefinition() == typeof(Nullable<>));
    }

    public override void WriteJson(JsonWriter writer, object value, JsonSerializer serializer)
    {
        if (value == null)
        {
            writer.WriteNull();
            return;
        }

        bool bValue = (bool)value;

        if (bValue)
        {
            writer.WriteValue(arrBString[0]);
        }
        else
        {
            writer.WriteValue(arrBString[1]);
        }
    }
}
```
 自定义了BoolConvert类型，继承自JsonConverter。构造函数参数BooleanString可以让我们自定义将true false转换成相应字符串。下面看实体里面怎么使用这个自定义转换类型

```csharp
public class Person
{
    [JsonConverter(typeof(BoolConvert))]
    public bool IsMarry { get; set; }
}
```

相应的有什么个性化的转换需求，都可以使用自定义转换类型的方式实现。

十.全局序列化设置

文章开头提出了Null值字段怎么不返回的问题，相应的在高级用法也给出了相应的解决方案使用
jsetting.NullValueHandling = NullValueHandling.Ignore; 
来设置不返回空值。这样有个麻烦的地方，每个不想返回空值的序列化都需设置一下。可以对序列化设置一些默认值方式么？下面将解答

```csharp
Newtonsoft.Json.JsonSerializerSettings setting = new Newtonsoft.Json.JsonSerializerSettings();
JsonConvert.DefaultSettings = new Func<JsonSerializerSettings>(() =>
{
　　　　//日期类型默认格式化处理
　　setting.DateFormatHandling = Newtonsoft.Json.DateFormatHandling.MicrosoftDateFormat;
    setting.DateFormatString = "yyyy-MM-dd HH:mm:ss";

　　　 //空值处理
    setting.NullValueHandling = NullValueHandling.Ignore;

    //高级用法九中的Bool类型转换 设置
    setting.Converters.Add(new BoolConvert("是,否"));

    return setting;
});
```
 