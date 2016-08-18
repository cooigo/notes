# 时间帮助类

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Web;

namespace Exchange.iFengTimes.Web.Util
{
    public class TimeHelper
    {
        /// <summary>
        /// 当前时间取距离1970.01.01的时间戳
        /// (表示自1970年1月1日00:00:00以来已经过的时间的以1毫秒为间隔的间隔数)
        /// 换算：1秒=1000毫秒
        /// 例子：以时间2011-07-25 00:00:00来计算
        /// 算出的时间戳为1311552000000   
        /// </summary>
        /// <param name="dt"></param>
        /// <returns></returns>
        public static long GetTimeTicks()
        {
            return GetTimeTicks(DateTime.Now);
        }

        /// <summary>
        /// 某传入时间取距离1970.01.01的时间戳
        /// (表示自1970年1月1日00:00:00以来已经过的时间的以1毫秒为间隔的间隔数)
        /// 换算：1秒=1000毫秒
        /// 例子：以时间2011-07-25 00:00:00来计算
        /// 算出的时间戳为1311552000000   
        /// </summary>
        /// <param name="dt"></param>
        /// <returns></returns>
        public static long GetTimeTicks(DateTime dt1)
        {
            DateTime dt1970 = new DateTime(1970, 1, 1);
            TimeSpan ts = dt1.Subtract(dt1970);
            return ts.Ticks / 10000;
        }
        /// <summary>
        /// 通过计算的时间戳，取得具体时间
        /// </summary>
        /// <param name="ticks"></param>
        /// <returns></returns>
        public static DateTime GetTimeFromTicks(long ticks)
        {
            DateTime dt1970 = new DateTime(1970, 1, 1);
            return new DateTime(ticks * 10000 + dt1970.Ticks);
        }
        /// <summary>
        /// 转换Int类型的Unix时间戳到标准的时间格式（当前时区）
        /// </summary>
        /// <param name="timeStamp"></param>
        /// <returns></returns>
        public static DateTime UnixTimeToTime(string timeStamp)
        {
            //设置计算起始时间
            DateTime dtStart = TimeZone.CurrentTimeZone.ToLocalTime(new DateTime(1970, 1, 1));
            //将Unix时间戳转换为C#里面的Tick滴答数。
            //Unix时间戳是从UTC（本初子午线）时间的1970年1月1日午夜0点算起到某个后来的时间点所经过的秒数，这个整数以秒为单位。（计算时，前后时间都要使用UTC时间）
            //C#里面的滴答（Ticks）是一个时间间隔单位，即100纳秒（也叫毫微秒）。c#里面没有时间戳的概念，而是由“计时周期数”这个概念。跟Unix时间含义相同，只是其单位和起始时间点不同。c#里面的计时周期数指的是：从0001年1月1日午夜到这个时间的Ticks数（所经过的Ticks数）。
            //所以，把Unix时间戳表示的这些时间，加到1970年那个Start起始时间点上，就能得到该时间戳表示的真正UTC时间。再换算到当前时区即可。
            //或者，直接把Unix时间戳这些时间，加到1970年1月1日当前时区的时间上，就直接得到了该Unix时间戳代表的当前时区的时刻。
            //首先，把时间戳的秒为单位，转换为c#里的100纳秒的单位Ticks数目。只需要乘以10的7次方即可。因为1秒到纳秒是乘以10的9次方，到百纳秒就是10的7次方。
            long lTime = long.Parse(timeStamp + "0000000");
            //使用这些时间间隔构造一个TimeSpan对象。
            TimeSpan toNow = new TimeSpan(lTime);
            //把TimeSpan对象，加到当前时区的1970起始时间上。
            return dtStart.Add(toNow);
        }
        /// <summary>
        /// 时间转换,将指定的时间，转换为一个linux时间戳---32位整型。
        /// </summary>
        /// <param name="time"></param>
        /// <returns></returns>
        public static long ConvertDateTimeToLong(System.DateTime time)
        {
            //获取计算Unix时间戳的1970年起始时间。（统一使用当前时区）
            System.DateTime startTime = TimeZone.CurrentTimeZone.ToLocalTime(new System.DateTime(1970, 1, 1));
            //拿当前时区的现在时间，减去1970起始时间，拿到这段时间的总秒数，就得到了时间戳
            return (long)(time - startTime).TotalSeconds;
        }
    }
}

```