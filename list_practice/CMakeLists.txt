# set(test) 这样初始化不太对
set(test "")
set(test2 "")

list(APPEND test "test")
list(APPEND test2 "abc")
message(WARNING ${test})

list(APPEND test "append")
list(APPEND test2 "hij")
message(STATUS ${test})

list(APPEND test "somethingwrong")
list(APPEND test2 "def")
message(STATUS ${test})
message(STATUS ${test2})

list(SORT test)
list(SORT test2)# sort不需要传值
message(STATUS ${test2})# message需要传值


# //message的警告等级从低到高
# //TRACE
# //DEBUG
# //VERBOSE
# //STATUS
# //NOTICE
# //WARNING
# //AUTRHOR_WARNING
# //SEND_ERROR
# //FATAL_ERROR

# //主要记住STATUS WARNING //FATAL_ERROR即可
