# [Code-Audit-Challenges](https://github.com/CHYbeta/Code-Audit-Challenges)

## 说明
一些有趣的代码审计“小”题目。

1. 为代码审计新手/小白提供一些帮助,为CTF-Web-dog提供一些套路。
2. 暂时先告诉大家世上最好的语言有：
     1. [php](https://github.com/CHYbeta/Code-Audit-Challenges#php)
     2. [python](https://github.com/CHYbeta/Code-Audit-Challenges#python)
     3. [node-js](https://github.com/CHYbeta/Code-Audit-Challenges#node-js)
     4. [Ruby](https://github.com/CHYbeta/Code-Audit-Challenges#ruby)
3. 以后还想告诉大家：java等等也是最好的语言。
4. 会不断整理更新，删/换部分题目。

## 题目来源：
+ 各大CTF-OJ平台
+ 各大CTF赛事
+ 知识星球等知识分享平台公开部分
+ 师傅们的想象力

## 注意
题目中涉及的代码可能不足以直接支撑一个完整的环境，若要本地搭建模拟，请自行修改。

该repo仅就原代码处的有趣点/漏洞点提出说明以及相应的解答。若有好的题目欢迎提供。

---
## PYTHON 
* [Challenge 1](python/challenge-1.md)：哈希长度扩展攻击

## Node-js
* [Challenge 1](node-js/challenge-1.md)：文件读取，URL处理

## Ruby
* [Challenge 1](ruby/challenge-1.md)：SQL注入

## PHP
* [Challenge 1](php/challenge-1.md)：phpBug #69892
* [Challenge 2](php/challenge-2.md)：php弱类型、is_numeric（）、强制类型转换
* [Challenge 3](php/challenge-3.md)：php配置文件写入问题
* [Challenge 4](php/challenge-4.md)
* [Challenge 5](php/challenge-5.md)：webshell、waf绕过
* [Challenge 6](php/challenge-6.md)：命令执行、waf绕过
* [Challenge 7](php/challenge-7.md)：php弱类型
* [Challenge 8](php/challenge-8.md)：SQL注入
* [Challenge 9](php/challenge-9.md)：php Session 序列化问题
* [Challenge 10](php/challenge-10.md)：php://input、php弱类型、eregi
* [Challenge 11](php/challenge-11.md)：SQL注入
* [Challenge 12](php/challenge-12.md)：命令执行
* [Challenge 13](php/challenge-13.md)：php弱类型、strcmp比较、ereg
* [Challenge 14](php/challenge-14.md)：SQL注入
* [Challenge 15](php/challenge-15.md)：php弱类型
* [Challenge 16](php/challenge-16.md)：SQL注入、逻辑漏洞
* [Challenge 17](php/challenge-17.md)：变量覆盖
* [Challenge 18](php/challenge-18.md)：SQL注入
* [Challenge 19](php/challenge-19.md)：SQL注入
* [Challenge 20](php/challenge-20.md)：SQL注入
* [Challenge 21](php/challenge-21.md)：stripos、php弱类型比较
* [Challenge 22](php/challenge-22.md)： 
* [Challenge 23](php/challenge-23.md)：变量覆盖
* [Challenge 24](php/challenge-24.md)：SQL注入
* [Challenge 25](php/challenge-25.md)：heredoc
* [Challenge 26](php/challenge-26.md)：php弱类型
* [Challenge 27](php/challenge-27.md)：php全局变量、$GLOBALS
* [Challenge 28](php/challenge-28.md)
* [Challenge 29](php/challenge-29.md)
* [Challenge 30](php/challenge-30.md)
* [Challenge 31](php/challenge-31.md)
* [Challenge 32](php/challenge-32.md)
* [Challenge 33](php/challenge-33.md)
* [Challenge 34](php/challenge-34.md)
* [Challenge 35](php/challenge-35.md)
* [Challenge 36](php/challenge-36.md)
* [Challenge 37](php/challenge-37.md)
* [Challenge 38](php/challenge-38.md)
* [Challenge 39](php/challenge-39.md)
* [Challenge 40](php/challenge-40.md)
* [Challenge 41](php/challenge-41.md)
* [Challenge 42](php/challenge-42.md)
* [Challenge 43](php/challenge-43.md)
* [Challenge 44](php/challenge-44.md)
* [Challenge 45](php/challenge-45.md)
* [Challenge 46](php/challenge-46.md)
* [Challenge 47](php/challenge-47.md)
* [Challenge 48](php/challenge-48.md)
* [Challenge 49](php/challenge-49.md)：哈希长度扩展攻击
* [Challenge 50](php/challenge-50.md)：SQL注入
* [Challenge 51](php/challenge-51.md)
* [Challenge 52](php/challenge-52.md)
* [Challenge 53](php/challenge-53.md)
* [Challenge 54](php/challenge-54.md)：Padding Oracle
* [Challenge 55](php/challenge-55.md)：SSRF
* [Challenge 56](php/challenge-56.md)：SQL注入
* [Challenge 57](php/challenge-57.md)
* [Challenge 58](php/challenge-58.md)
* [Challenge 59](php/challenge-59.md)：hash碰撞
* [Challenge 60](php/challenge-60.md)：命令执行
* [Challenge 61](php/challenge-61.md)：SSRF





## 分类
+ SQL注入
    + PHP:
        + [Challenge 8](php/challenge-8.md)
        + [Challenge 11](php/challenge-11.md)
        + [Challenge 14](php/challenge-14.md)
        + [Challenge 16](php/challenge-16.md)
        + [Challenge 18](php/challenge-18.md) 
        + [Challenge 19](php/challenge-19.md) 
        + [Challenge 20](php/challenge-20.md)
        + [Challenge 24](php/challenge-24.md)
        + [Challenge 50](php/challenge-50.md)
    + Ruby:
        + [Challenge 1](ruby/challenge-1.md)
+ 命令执行
    + PHP:
        + [Challenge 4](php/challenge-4.md)
        + [Challenge 6](php/challenge-6.md)
        + [Challenge 12](php/challenge-12.md)
        + [Challenge 60](php/challenge-60.md)：命令执行

+ 弱类型比较等
    + PHP:
        + [Challenge 1](php/challenge-1.md)
        + [Challenge 2](php/challenge-2.md)
        + [Challenge 7](php/challenge-7.md)
        + [Challenge 10](php/challenge-10.md)
        + [Challenge 13](php/challenge-13.md)
        + [Challenge 15](php/challenge-15.md)
        + [Challenge 21](php/challenge-21.md)
        + [Challenge 26](php/challenge-26.md)
+ 反序列化
    + PHP:
        + [Challenge 9](php/challenge-9.md)
+ 变量覆盖
    + PHP:
        + [Challenge 17](php/challenge-17.md)
        + [Challenge 23](php/challenge-23.md)
+ 密码学相关
    + PHP:
        + [Challenge 49](php/challenge-49.md)
        + [Challenge 54](php/challenge-54.md)
        + [Challenge 59](php/challenge-59.md)
    + PYTHON:
        + [Challenge 1](python/challenge-1.md)
+ 其他
    + PHP:
        + [Challenge 1](php/challenge-1.md)
        + [Challenge 3](php/challenge-3.md)
        + [Challenge 5](php/challenge-5.md)
        + [Challenge 25](php/challenge-25.md)
        + [Challenge 27](php/challenge-27.md)
        + [Challenge 49](php/challenge-49.md)
        
    + Node-js:
        + [Challenge 1](node-js/challenge-1.md)
---



