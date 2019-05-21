## Algorithm
题目：删除最外层的括号
有效括号字符串为空 ("")、"(" + A + ")" 或 A + B，其中 A 和 B 都是有效的括号字符串，+ 代表字符串的连接。例如，""，"()"，"(())()" 和 "(()(()))" 都是有效的括号字符串。
如果有效字符串 S 非空，且不存在将其拆分为 S = A+B 的方法，我们称其为原语（primitive），其中 A 和 B 都是非空有效括号字符串。
给出一个非空有效字符串 S，考虑将其进行原语化分解，使得：S = P_1 + P_2 + ... + P_k，其中 P_i 是有效括号字符串原语。
对 S 进行原语化分解，删除分解中每个原语字符串的最外层括号，返回 S 。

思路分析，按照题目信息，左右括号必然时对应关系。一一对应的情况，假设一个特征值tag，左侧括号特征值+1，右侧括号特征值-1，最终一个完整的原语（primitive）其特征值tag=0（当然其他的值也可以，必然是一个特定的值）
第一版本：验证方案是否可行

    class Solution {
        public String removeOuterParentheses(String s) {
            if ("".equals(s)) {
                return "";
            }
        
            int tag = 0;
            int subStart = 0;
            int subEnd = 0;
            String[] pArr = s.split("");
            StringBuilder sb = new StringBuilder();
        
            for (int index = 0; index < pArr.length; index++) {
                String p = pArr[index];
            
                if ("(".equals(p)) {
                    tag += 1;
                } else if (")".equals(p)) {
                    tag -= 1;
                }
            
                if (tag == 0) {
                    subEnd = index;
                    if (subStart + 1 >= subEnd) {
                        sb.append("");
                    } else {
                        sb.append(s.substring(subStart + 1, subEnd));
                    }
                    subStart = index+1;
                }
            }
            return sb.toString();
        
        }
    }

第二版本：解决犯傻的问题，比如不创建零时字符串、不创建字符串数组
主要代码变动如下，这一版本改变后执行时间由97ms降低到7ms，作为参考

    for (int index = 0; index < s.length(); index++) {
            if ('(' == s.charAt(index)) {
                tag += 1;
            } else if (')' == s.charAt(index)) {
                tag -= 1;
            }
	...

第三版本：在比较的过程中添加字符，不再通过substring方法获取。观察下面的测试用例
((())) => 1 2 3 2 1 0
可以发现 '(' 在 tag>1 时即可添加，')' 在 tag > 0 时即可添加，正好将外层括号去掉。测试执行时间
再次降低到2ms

    private static String removeOuterParentheses(String s) {
        int tag = 0;
        char[] cArr = s.toCharArray();
        StringBuilder sb = new StringBuilder();
        for (char c : cArr) {
            if ('(' == c) {
                ++tag;
                if (tag > 1) {
                    sb.append(c);
                }
            } else {
                --tag;
                if (tag > 0) {
                    sb.append(c);
                }
            }
        }
        return sb.toString();
    }

## Review
阅读文章为[BLoC pattern in Flutter](https://medium.com/flutterdevs/bloc-pattern-in-flutter-part-1-flutterdevs-128f90059f5c)

BLoC主要解决代码复用的场景，通过Stream组织业务逻辑，使UI组件对业务逻辑无感知。
Stream可以理解为管道，两个开口，一个input一个output。UI组件对业务逻辑不再关心，只通过input触发
业务逻辑，然后再output拿到数据变化，进行UI组件的重建展示。
这样，整体BLoC代码再Stream中是独立的，仅仅包括业务代码，而不关心具体的平台。
Google将Flutter发展为全平台开发环境后，BLoC可以复用再所有支持的平台，包括Web、客户端。

参考链接
[Flutter / AngularDart – Code sharing, better together (DartConf 2018)](https://www.youtube.com/watch?v=PLHln7wHgPE)

## Tip
Flutter通过pubspec.yaml来管理assets资源，assets资源创建时需要有一个前导符号：'-'，该Tip就是前天遇到的一个实际问题。

    assets:
        - images/ic_nav_back.png

在添加资源时，如果不小心多了一个'-'，编译就会错误，错误信息如下：

    Flutter crash report; please file at https://github.com/flutter/flutter/issues.

    command
    flutter build bundle --suppress-analytics --target lib/main.dart --compilation-trace-file compilation.txt     --target-platform android-arm --precompiled --asset-dir /Users/bobzhai/FlutterProjects/    AcewillManager/acewill_manager/build/app/intermediates/flutter/release/flutter_assets --release

    exception
    NoSuchMethodError: NoSuchMethodError: The getter 'length' was called on null.
    Receiver: null
    Tried calling: length

    #0      Object.noSuchMethod (dart:core-patch/object_patch.dart:50:5)
    #1      _Uri._uriEncode (dart:core-patch/uri_patch.dart:44:23)
    #2      Uri.encodeFull (dart:core/uri.dart:1145:17)
    #3      MappedListIterable.elementAt (dart:_internal/iterable.dart:414:29)
    #4      MappedListIterable.elementAt (dart:_internal/iterable.dart:414:40)
    #5      ListIterable.toList (dart:_internal/iterable.dart:219:19)
    #6      FlutterManifest.assets (package:flutter_tools/src/flutter_manifest.dart:182:11)
    #7      _parseAssets (package:flutter_tools/src/asset.dart:558:40)
    ...

这一段有点困扰我，NoSuchMethodError: NoSuchMethodError: The getter 'length' was called on null. 获取不到资源尽然报错NoSuchMethodError。
应该理解为，获取资源时资源为null。

## Share
[RxDart与BLoC Pattern在Flutter开发时怎么结合](https://medium.com/flutter-community/why-use-rxdart-and-how-we-can-use-with-bloc-pattern-in-flutter-a64ca2c7c52d)
不要无脑的堆砌代码，使用更优雅、可维护、可扩展的方式
