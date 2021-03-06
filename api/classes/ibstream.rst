ibstream_view 和 ibstream
^^^^^^^^^^^^^^^^^^^^^^^^^^
二进制读取流，ibstream_view借鉴C++17标准引入的 ``std::string_view`` 无GC读取二进制数据，
读取数值类型时自动转换为主机字节序， 如果需要使用深拷贝对象，使用ibstream即可，
ibstream继承了所有ibstream_view关于二进制读取的接口。


命名空间
---------------------
``namespace yasio``

成员
-----------------
.. list-table:: 
   :widths: auto
   :header-rows: 1

   * - 函数名
     - 函数说明
   * - ibstream_view::ibstream_view
     - 构造ibstream_view对象

公共方法:

.. list-table:: 
   :widths: auto
   :header-rows: 1

   * - 函数名
     - 函数说明
   * - ibstream_view::reset
     - 重置输入数据
   * - ibstream_view::read_ix
     - 读取7bit Encoded Int变长存储, 函数模板，支持int和int64_t
   * - ibstream_view::read_byte
     - 读取一个字节
   * - ibstream_view::read
     - 函数模板，读取数值类型数据，会自动转化为主机字节序
   * - ibstream_view::read_v
     - 读取字节流数据，会先读取7bit编码的变长长度域
   * - ibstream_view::read_bytes
     - 读取字节流数据，无长度域
   * - ibstream_view::seek
     - 移动流指针，和系统文件io lseek用法相同
   * - ibstream_view::length
     - 获取stream总字节数
   * - ibstream_view::data
     - 获取字节数据指针
   * - ibstream_view::sread
     - 静态函数模板，在指定内存地址读取数值数据，会自动转化为网络字节序
   * - ibstream_view::read_v32
     - 读取字节流数据，使用32bit长度域 (不常用)
   * - ibstream_view::read_v16
     - 读取字节流数据，使用16bit长度域 (不常用)
   * - ibstream_view::read_v8
     - 读取字节流数据，使用8bit长度域 (不常用)
   * - ibstream_view::read_i24
     - 读取24位有符号整数 (不常用)
   * - ibstream_view::read_u24
     - 读取24位无符号整数 (不常用)


Example
--------------------------
.. tabs::
 .. code-tab:: cpp

  using namespace yasio;
  ibstream_view ibs(event->packet().data(), event->packet().size());
  
  // 读取1字节整数
  int8_t cmd = ibs.read<int8_t>();
  
  // 跳过4字节长度域
  ibs.seek(4, SEEK_CUR);
  
  int16_t seq = ibs.read<int16_t>();
  cxx17::string_view msg1 = ibs.read_v16();
  cxx17::string_view msg2 = ibs.read_v();
