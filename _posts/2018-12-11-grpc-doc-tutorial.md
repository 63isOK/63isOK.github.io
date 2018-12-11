# c++使用grpc
  总共3步：
* proto文件中定义一个service
* pb编译proto文件，生成c++的client、server代码
* 基于上面定义的service，使用c++ grpc api实现一个小例子

  .pb.h .pb.cc message的头文件和源文件
  .grpc.pb.h .grpc.pb.cc service的头文件的和源文件

