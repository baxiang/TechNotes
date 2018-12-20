SDK的key、网络加密使用的“盐”等字符串，不能使用明文保存，需要对这些静态字符串进行加密，以防止在APP被反编译后泄露。然后在需要使用字符串的地方进行解密。通常我们使用异或加密来加密字符串.
异或的运算方法是一个二进制运算： 1^1=0 0^0=0 1^0=1 0^1=1
两者相等为0,不等为1.
对于一个字符来说,都可以用二进制码来表示.如A:01000001，字符的异或就是对每一位进行二进制运算. 用于加密算法时,假设你要加密的内容为A,密钥为B,则可以用
异或加密:C=A^B 在数据中保存加密后的C就行了. 用的时候:A=B^C 即可取得原加密A的内容,所以只要知道密钥,就可以完成加密和解密.
异或加密和解密的方法一致，运算一次就是加密，再运算一次就是解密。
```
- (NSString *)xorStr:(NSString *)xorStr withKey:(NSString *)key{
    NSData  *strData = [xorStr dataUsingEncoding:NSUTF8StringEncoding];
    NSData *keyData = [key dataUsingEncoding:NSUTF8StringEncoding];
    Byte *keyBytes = (Byte *)[keyData bytes];   //取关键字的Byte数组, keyBytes一直指向头部
    Byte *sourceDataPoint = (Byte *)[strData bytes];  //取需要异或的数据的Byte数组
    for (long i = 0; i < [strData length]; i++) {
        sourceDataPoint[i] = sourceDataPoint[i] ^ keyBytes[(i % [keyData length])]; //然后按位进行异或运算
    }
    NSString *outStr = [[NSString alloc]initWithData:strData encoding:NSUTF8StringEncoding];
    return outStr;
}
```
