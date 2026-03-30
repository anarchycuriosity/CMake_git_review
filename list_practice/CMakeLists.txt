# set(test) 这样初始化不太对
set(test "")
list(APPEND test "test")
message(WARNING ${test})
list(APPEND test "append")
message(SEND_ERROR ${test})