## 属性路由

|Constraint|Description|Example|
|----|----|----|
|alpha      |Matchesuppercase or lowercase Latin alphabet characters (a-z, A-Z)|{x:alpha}|
|bool       |Matches a Boolean value.|{x:bool}|
|datetime   |Matches a DateTime value.|{x:datetime}|
|decimal    |Matches a decimal value.|{x:decimal}|
|double     |Matches a 64-bit floating-point value.|{x:double}|
|float      |Matches a 32-bit floating-point value.|{x:float}|
|guid       |Matches a GUID value.|	{x:guid}|
|int        |Matches a 32-bit integer value.|{x:int}|
|length     |Matches a string with the specified length or within a specified range of lengths. |{x:length(6)} {x:length(1,20)}|
|long       |Matches a 64-bit integer value.|{x:long}|
|max        |Matches an integer with a maximum value.|{x:max(10)}|
|maxlength  |Matches a string with a maximum length.|{x:maxlength(10)}|
|min        |Matches an integer with a minimum value.|{x:min(10)}|
|minlength  |Matches a string with a minimum length.|{x:minlength(10)}|
|range      |Matches an integer within a range of values.|{x:range(10,50)}|
|regex      |Matches a regular expression.|{x:regex(^\d{3}-\d{3}-\d{4}$)}|