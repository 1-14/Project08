# Project8

通过网上查阅资料，我找到了三种基于ARM64指令的AES软件实现

## aes_scalar.cpp

使用标量实现，除了ARM也可以在其他平台上编译实现

### ARM指令功能及用途

**ROR8(w): 右循环移位指令**

功能：将32位无符号整数w右循环移动8位。

用途：在AES中，用于将字中的字节循环移动，是加密和解密过程中的重要步骤之一。

**sub_bytes(uint32_t word): S-Box替换指令**

功能：将32位无符号整数word中的每个字节通过查找表进行S-Box替换。

用途：在AES加密和解密过程中，用于进行字节替换，增加密码的混淆度。

**inv_sub_bytes(uint32_t word): 逆S-Box替换指令**

功能：将32位无符号整数word中的每个字节通过逆查找表进行逆S-Box替换。

用途：在AES解密过程中，用于逆向字节替换，还原原始数据。

**shift_rows(uint8_t *state)**: **行移位指令**

功能：将AES状态数组（4x4字节）中的每一行进行循环移位操作。

用途：在AES加密过程中，用于行移位，增加密码的扩散性。

**inv_shift_rows(uint8_t *state)**: **逆行移位指令**

功能：将AES状态数组（4x4字节）中的每一行进行逆循环移位操作。

用途：在AES解密过程中，用于逆向行移位，还原原始数据。

**mix_columns(uint32_t word): 列混淆指令**

功能：将32位无符号整数word中的每个字节进行列混淆操作。

用途：在AES加密过程中，用于混淆列，增加密码的扩散性。

**inv_mix_columns(uint32_t word): 逆列混淆指令**

功能：将32位无符号整数word中的每个字节进行逆列混淆操作。

用途：在AES解密过程中，用于逆向列混淆，还原原始数据。

**add_round_key(uint32_t *state, uint32_t *key)**: **轮密钥加指令**

功能：将当前状态数组state与轮密钥数组key进行按字节异或操作。

用途：在AES加密和解密过程中，用于将当前状态与轮密钥进行混合。

**gen_keys(const uint8_t *input_key, uint8_t *keys, const uint16_t keylen)**: **生成轮密钥指令**

功能：根据输入密钥生成AES加密和解密过程所需的轮密钥。

用途：在AES加密和解密过程中，用于生成轮密钥。

**encryption_round(uint32_t *state, uint32_t *key)**: **加密轮指令**

功能：对AES状态数组state进行一轮加密操作。

用途：在AES加密过程中，对数据进行一轮加密。

**decryption_round(uint32_t *state, uint32_t *key)**: **解密轮指令**

功能：对AES状态数组state进行一轮解密操作。

用途：在AES解密过程中，对数据进行一轮解密。

**encrypt(uint8_t *input, uint8_t *output, uint8_t *keys, uint8_t numkeys)**: **加密指令**

功能：使用AES算法对输入数据input进行加密。

用途：在AES加密过程中，用于加密数据块。

**decrypt(uint8_t *input, uint8_t *output, uint8_t *keys, uint8_t numkeys)**: **解密指令**

功能：使用AES算法对输入数据input进行解密。

用途：在AES解密过程中，用于解密数据块。

## aes_neon.cpp

使用ARMv8的NEON SIMD指令实现

## aes_bitslice.cpp

此实现使用位切片算法，该算法允许并行处理 8 个加密/解密。 仅支持 128 位和 256 位密钥。

## 实现效果

使用 gcc 11.3.0 for Ubuntu 编译，使用 -O3 优化级别。 在 11代Intel Core i7 CPU上执行。 每个基准测试包括在 100 个数据块上完成的8000000轮加密。

aes_scalar：22 秒

aes_neon：16 秒

aes_bitslice：12 秒
