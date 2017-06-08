##spark日志处理之时间字段处理

日志文件中的时间大多被保存为string类型，处理时很不方便，这里提供一个处理方式。

#####１.转换为时间戳处理

自己编写函数将string字段转化为时间戳;注意写好时间的格式
```
import Java.sql.Timestamp
import java.text.SimpleDateFormat
import java.util.Date
 /*
 * change date string to timestamp value
 */
 def getTimestamp(x:String) :java.sql.Timestamp = {
 //"20151021235349"
   val format = new SimpleDateFormat("yyyyMMddHHmmss")
   var ts = new Timestamp(System.currentTimeMillis());
   try {
     if (x == "")
      return null
        else {
          val d = format.parse(x);
          val t = new Timestamp(d.getTime());
          return t
            }
        } catch {
          case e: Exception => println("cdr parse timestamp wrong")
         }
        return null
    }
    ```
#####2.scala处理时间戳

我们可以调用java中处理时间戳的工具来完成对时间的处理：

    package com.yh.hbaseusers
    import java.time.LocalDate
    /**  
      * Created by silentwolf on 2016/6/30.  
      */  
    object fileTest {  
      def main(args: Array[String]) {  
         var nowdate = LocalDate.now()  
         println("LocalDate.now()-->当前时间"+LocalDate.now())  
         println("nowdate.plusDays-->明天-->"+nowdate.plusDays(1))  
         println("nowdate.minusDays-->昨天-->"+nowdate.minusDays(1))  
         println("nowdate.plusMonths-->当前加一个月-->"+nowdate.plusMonths(1))  
         println("nowdate.minusMonths-->当前减一个月-->"+nowdate.minusMonths(1))  
         println("nowdate.getDayOfYear-->今天是今年的第"+nowdate.getDayOfYear+"天")  
         println("nowdate.getDayOfMonth->这个月有"+nowdate.getDayOfMonth+"天")  
         println("nowdate.getDayOfWeek-->今天星期"+nowdate.getDayOfWeek)  
         println("nowdate.getMonth-->这个月是"+nowdate.getMonth)  
        }  
    }  
    
#####直接处理的方法

如果没有在处理之前将时间字段转化为时间戳，也可以直接进行处理（在函数内转化）

    //核心工作时间，迟到早退等的的处理  
    def getCoreTime(start_time:String,end_Time:String)={  
    var df:SimpleDateFormat=new SimpleDateFormat("HH:mm:ss")  
    var begin:Date=df.parse(start_time)  
    var end:Date = df.parse(end_Time)  
    var between:Long=(end.getTime()-begin.getTime())/1000//转化成秒  
    var hour:Float=between.toFloat/3600  
    var decf:DecimalFormat=new DecimalFormat("#.00")  
    decf.format(hour)//格式化    
    }  
