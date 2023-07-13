# Project8

通过网上查阅资料，我找到了三种基于ARM64指令的AES软件实现

## aes_scalar.cpp

使用标量实现，除了ARM也可以在其他平台上编译实现

## aes_neon.cpp

使用ARMv8的NEON SIMD指令实现

## aes_bitslice.cpp

此实现使用位切片算法，该算法允许并行处理 8 个加密/解密。 仅支持 128 位和 256 位密钥。

## 实现效果

使用 gcc 11.3.0 for Ubuntu 编译，使用 -O3 优化级别。 在 11代Intel Core i7 CPU上执行。 每个基准测试包括在 100 个数据块上完成的8000000轮加密。

aes_scalar：22 秒
aes_neon：16 秒
aes_bitslice：12 秒
