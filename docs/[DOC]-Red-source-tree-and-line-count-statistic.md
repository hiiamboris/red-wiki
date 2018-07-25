## How many lines did Red coded?

Base on Red `0.6.3` version, exclude directorys whose name contain `test`, `docs`, `sample` and `assets`, and files whose name start with `.` like `.DS_Store`, `.travis.yml`, `.editorconfig` and `.appveyor.yml`.

**Note: Lines starts with `;` and empty lines will not be count in.**

```shell
-------Total line count of each file type------
     .reds: 74162
        .r: 22072
      .red: 12708
      .svg: 377
       .md: 225
      .bas: 204
        .h: 178
     .java: 128
      .lib: 103
      .txt: 97
      .def: 63
      .dex: 30
   .sample: 15
```

## The most important part in my opinion?

`Red` is built with `Red/System` low level(same level as `C`) language: the **runtime** and **datatypes**, which are the foundation of Red.

And currently compilers of `Red` and `Red/System` are all written with `REBOL`, but compiler is so complicated, not to mention the linking and emitting to machine code, in which phases depend on different OSs and different CPUs.

This article "[Red/System compiler overview](http://www.red-lang.org/2011/05/redsystem-compiler-overview.html)" from `Red` official site is worth reading.

After [I compiled a very simple `Red` code to `Red/System`](https://github.com/red/red/wiki/Compile-Red-code-to-Red-System-and-learn-something), I found maybe these three files are the startpoint of `Red`.

```shell
...compilation time : 51 ms
[
    Red/System [origin: 'Red] 
    red: context [
        #include %/Users/hao/test/runtime/definitions.reds 
        #include %/Users/hao/test/runtime/macros.reds 
        #include %/Users/hao/test/runtime/datatypes/structures.reds 
    ...
]
```

So I will try to read the `Red` runtime code below, to understand the `Red` inside.

```shell
>> python cloc.py red-0.6.3/runtime/

Output:

-----File name-----------------------Line count
red-0.6.3/runtime
|__utils.reds                               161
|__unicode.reds                             724
|__tools.reds                                27
|__stack.reds                               664
|__sort.reds                                413
|__simple-io.reds                          1915
|__redbin.reds                              463
|__red.reds                                 248
|__random.reds                               86
|__parse.reds                              1728
|__ownership.reds                           216
|__natives.reds                            2908
|__macros.reds                              512
|__interpreter.reds                         962
|__hashtable.reds                           915
|__definitions.reds                         301
|__debug-tools.reds                         228
|__crypto.reds                              587
|__crush.reds                               453
|__clipboard.reds                           219
|__case-folding.reds                       1277
|__call.reds                                715
|__allocator.reds                           634
|__actions.reds                            1544
|
|__+datatypes/
|  |__word.reds                             486
|  |__vector.reds                          1087
|  |__url.reds                              342
|  |__unset.reds                            179
|  |__typeset.reds                          483
|  |__tuple.reds                            650
|  |__time.reds                             462
|  |__tag.reds                              116
|  |__symbol.reds                           216
|  |__structures.reds                       271
|  |__string.reds                          2439
|  |__set-word.reds                         156
|  |__set-path.reds                         132
|  |__series.reds                          1105
|  |__routine.reds                          215
|  |__refinement.reds                       153
|  |__point.reds                            131
|  |__percent.reds                          186
|  |__path.reds                             251
|  |__paren.reds                            122
|  |__pair.reds                             466
|  |__op.reds                               252
|  |__object.reds                          1344
|  |__none.reds                             238
|  |__native.reds                           305
|  |__map.reds                              706
|  |__logic.reds                            329
|  |__lit-word.reds                         155
|  |__lit-path.reds                         130
|  |__issue.reds                            135
|  |__integer.reds                          744
|  |__image.reds                            889
|  |__hash.reds                             220
|  |__handle.reds                           169
|  |__get-word.reds                         158
|  |__get-path.reds                         130
|  |__function.reds                        1006
|  |__float.reds                            881
|  |__file.reds                             308
|  |__event.reds                            184
|  |__error.reds                            380
|  |__email.reds                            150
|  |__date.reds                             986
|  |__datatype.reds                         226
|  |__context.reds                          538
|  |__common.reds                           765
|  |__char.reds                             267
|  |__block.reds                           1599
|  |__bitset.reds                           901
|  |__binary.reds                          1131
|  |__action.reds                           181
|
|__+platform/
|  |__win32.reds                            472
|  |__win32-gui.reds                        108
|  |__win32-cli.reds                        270
|  |__win32-ansi.reds                       381
|  |__syllable.reds                          72
|  |__POSIX.reds                            451
|  |__linux.reds                             98
|  |__image-quartz.reds                     661
|  |__image-gdiplus.reds                    587
|  |__freebsd.reds                           79
|  |__darwin.reds                            82
|  |__COM.reds                              318
|  |__android.reds                          123

-------Total line count of each file type------
     .reds: 46657
```

## Python script to count code lines

```python
# -*- coding: utf-8 -*-

import os
import sys

def count_path(path, layer=0, basedir=None):
    if os.path.isfile(path):
        _, ext = os.path.splitext(path)
        fname = os.path.basename(path)
        if fname.startswith('.'):
            return {}

        with open(path) as f:
            linecnt = 0
            for line in f.xreadlines():
                if len(line.strip()) == 0:
                    continue
                elif line.strip().startswith(';'):
                    continue
                linecnt += 1

            dic = dict({ext: linecnt})
            s = '|' + '  |'*(layer-1) + '__' + fname
            linecnt = str(linecnt).rjust(46-len(s), ' ')
            print '%s %s' % (s, linecnt)
            return dic

    elif os.path.isdir(path):
        if 'test' in path or 'docs' in path or 'sample' in path or 'assets' in path:
            return {}

        dir_dic = {}
        if layer == 0:
            print path
        else:
            print '|' + '  |'*(layer-1)
            #print '|' + '  |'*(layer-1) + '__+' + os.path.basename(path) + '/'
            print '|' + '  |'*(layer-1) + '__+' + path.replace(basedir, '')[1:] + '/'

        reorder = []
        for i in os.listdir(path):
            i = os.path.join(path, i)
            if os.path.isdir(i):
                reorder.append(i)
            else:
                reorder.insert(0, i)

        for i in reorder:
            item_dic = count_path(i, layer=layer+1, basedir=basedir)
            if not item_dic:
                continue

            for ext, linecnt in item_dic.items():
                if ext not in dir_dic.keys():
                    dir_dic.update({ext: linecnt})
                else:
                    dir_dic[ext] += linecnt

        return dir_dic


if __name__ == '__main__':
    if len(sys.argv) == 1:
        print 'Usage: python cloc.py <dir>'
        sys.exit(0)

    else:
        dir = sys.argv[1]
        # dir = os.path.abspath(path)
        if not os.path.isdir(dir):
            print '[%s] is not a valid dir'
            sys.exit(0)

        print '\n-----File name-----------------------Line count'
        dic = count_path(dir, basedir=dir)

        ordered_dic = sorted([ (v, k) for k, v in dic.iteritems() ], reverse=True)

        print '\n-------Total line count of each file type------'
        for linecnt, ext in ordered_dic:
            s = ext.rjust(10, ' ')
            print '%s: %s' % (s, linecnt)

```

## The whole Red source tree and line count

```shell
>> python cloc.py red-0.6.3

Output:

-------Total line count of each file type------
     .reds: 74162
        .r: 22072
      .red: 12708
      .svg: 377
       .md: 225
      .bas: 204
        .h: 178
     .java: 128
      .lib: 103
      .txt: 97
      .def: 63
      .dex: 30
   .sample: 15

-----File name-----------------------Line count
red-0.6.3
|__version.r                                  1
|__usage.txt                                 53
|__run-all.r                                 98
|__red.r                                    757
|__README.md                                131
|__lexer.r                                  759
|__compiler.r                              4224
|__BSL-License.txt                           20
|__BSD-3-License.txt                         22
|__boot.red                                  46
|
|__+bridges/
|  |
|  |__+bridges/android/
|  |  |__build.r                            143
|  |  |
|  |  |__+bridges/android/dex/
|  |  |  |__classes.dex                      30
|  |  |
|  |  |__+bridges/android/src/
|  |  |  |
|  |  |  |__+bridges/android/src/org/
|  |  |  |  |
|  |  |  |  |__+bridges/android/src/org/redlang/
|  |  |  |  |  |
|  |  |  |  |  |__+bridges/android/src/org/redlang/eval/
|  |  |  |  |  |  |__TouchEvent.java          9
|  |  |  |  |  |  |__MainActivity.java       18
|  |  |  |  |  |  |__LongClickEvent.java      8
|  |  |  |  |  |  |__ClickEvent.java          8
|  |  |  |
|  |  |  |__+bridges/android/src/redroid/
|  |  |  |  |__TouchEvent.java                9
|  |  |  |  |__MainActivity.java             18
|  |  |  |  |__LongClickEvent.java            8
|  |  |  |  |__ClickEvent.java                8
|  |
|  |__+bridges/dotnet/
|  |  |__test.svg                           377
|  |  |__test-wpf.red                        36
|  |  |__test-svg.red                        32
|  |  |__README.md                           30
|  |  |__bridge.red                         829
|  |
|  |__+bridges/java/
|  |  |__README.md                           24
|  |  |__hello.red                           37
|  |  |__bridge.red                         661
|  |  |__bridge.java                         23
|
|__+build/
|  |__README.md                              15
|  |__precap.r                               14
|  |__includes.r                            274
|  |__encap-paths.r.sample                   15
|  |__build.r                                61
|
|__+environment/
|  |__system.red                            304
|  |__scalars.red                            42
|  |__routines.red                           78
|  |__reactivity.red                        298
|  |__operators.red                          32
|  |__natives.red                           777
|  |__lexer.red                             934
|  |__functions.red                         896
|  |__datatypes.red                          61
|  |__css-colors.red                        151
|  |__colors.red                             56
|  |__actions.red                           508
|  |
|  |__+environment/codecs/
|  |  |__png.red                             27
|  |  |__jpeg.red                            27
|  |  |__gif.red                             27
|  |  |__bmp.red                             27
|  |
|  |__+environment/console/
|  |  |__windows.reds                       606
|  |  |__win32.reds                         305
|  |  |__wcwidth.reds                       180
|  |  |__terminal.reds                     1467
|  |  |__POSIX.reds                         464
|  |  |__input.red                          547
|  |  |__help.red                           436
|  |  |__gui-console.red                    246
|  |  |__engine.red                         260
|  |  |__console.red                         16
|  |  |__auto-complete.red                  183
|
|__+libRed/
|  |__red.h                                 178
|  |__README.md                               7
|  |__libRed.red                           1176
|  |__libRed.lib                            103
|  |__libRed.def                             63
|  |__libRed.bas                            204
|
|__+modules/
|  |
|  |__+modules/view/
|  |  |__view.red                          1001
|  |  |__VID.red                            636
|  |  |__utils.red                           65
|  |  |__styles.red                         110
|  |  |__rules.red                           11
|  |  |__draw.red                          1132
|  |  |
|  |  |__+modules/view/backends/
|  |  |  |__platform.red                    720
|  |  |  |__keycodes.reds                   185
|  |  |  |
|  |  |  |__+modules/view/backends/android/
|  |  |  |  |__gui.red                       84
|  |  |  |
|  |  |  |__+modules/view/backends/macOS/
|  |  |  |  |__text-box.reds                341
|  |  |  |  |__tab-panel.reds               143
|  |  |  |  |__selectors.reds                39
|  |  |  |  |__rules.red                    126
|  |  |  |  |__para.reds                    126
|  |  |  |  |__menu.reds                    197
|  |  |  |  |__gui.reds                    1823
|  |  |  |  |__font.reds                    249
|  |  |  |  |__events.reds                  633
|  |  |  |  |__draw.reds                   1772
|  |  |  |  |__delegates.reds              1173
|  |  |  |  |__comdlgs.reds                 412
|  |  |  |  |__cocoa.reds                   907
|  |  |  |  |__classes.reds                 204
|  |  |  |  |__camera.reds                  168
|  |  |  |
|  |  |  |__+modules/view/backends/windows/
|  |  |  |  |__win32.reds                  2679
|  |  |  |  |__text-list.reds               225
|  |  |  |  |__text-box.reds                388
|  |  |  |  |__tab-panel.reds               314
|  |  |  |  |__rules.red                     73
|  |  |  |  |__para.reds                    175
|  |  |  |  |__panel.reds                    42
|  |  |  |  |__menu.reds                    157
|  |  |  |  |__gui.reds                    2028
|  |  |  |  |__font.reds                    276
|  |  |  |  |__events.reds                 1230
|  |  |  |  |__draw.reds                   3235
|  |  |  |  |__draw-d2d.reds                162
|  |  |  |  |__direct2d.reds               1053
|  |  |  |  |__comdlgs.reds                 197
|  |  |  |  |__classes.reds                 227
|  |  |  |  |__camera.reds                  355
|  |  |  |  |__button.reds                   82
|  |  |  |  |__base.reds                    743
|
|__+runtime/
|  |__utils.reds                            161
|  |__unicode.reds                          724
|  |__tools.reds                             27
|  |__stack.reds                            664
|  |__sort.reds                             413
|  |__simple-io.reds                       1915
|  |__redbin.reds                           463
|  |__red.reds                              248
|  |__random.reds                            86
|  |__parse.reds                           1728
|  |__ownership.reds                        216
|  |__natives.reds                         2908
|  |__macros.reds                           512
|  |__interpreter.reds                      962
|  |__hashtable.reds                        915
|  |__definitions.reds                      301
|  |__debug-tools.reds                      228
|  |__crypto.reds                           587
|  |__crush.reds                            453
|  |__clipboard.reds                        219
|  |__case-folding.reds                    1277
|  |__call.reds                             715
|  |__allocator.reds                        634
|  |__actions.reds                         1544
|  |
|  |__+runtime/datatypes/
|  |  |__word.reds                          486
|  |  |__vector.reds                       1087
|  |  |__url.reds                           342
|  |  |__unset.reds                         179
|  |  |__typeset.reds                       483
|  |  |__tuple.reds                         650
|  |  |__time.reds                          462
|  |  |__tag.reds                           116
|  |  |__symbol.reds                        216
|  |  |__structures.reds                    271
|  |  |__string.reds                       2439
|  |  |__set-word.reds                      156
|  |  |__set-path.reds                      132
|  |  |__series.reds                       1105
|  |  |__routine.reds                       215
|  |  |__refinement.reds                    153
|  |  |__point.reds                         131
|  |  |__percent.reds                       186
|  |  |__path.reds                          251
|  |  |__paren.reds                         122
|  |  |__pair.reds                          466
|  |  |__op.reds                            252
|  |  |__object.reds                       1344
|  |  |__none.reds                          238
|  |  |__native.reds                        305
|  |  |__map.reds                           706
|  |  |__logic.reds                         329
|  |  |__lit-word.reds                      155
|  |  |__lit-path.reds                      130
|  |  |__issue.reds                         135
|  |  |__integer.reds                       744
|  |  |__image.reds                         889
|  |  |__hash.reds                          220
|  |  |__handle.reds                        169
|  |  |__get-word.reds                      158
|  |  |__get-path.reds                      130
|  |  |__function.reds                     1006
|  |  |__float.reds                         881
|  |  |__file.reds                          308
|  |  |__event.reds                         184
|  |  |__error.reds                         380
|  |  |__email.reds                         150
|  |  |__date.reds                          986
|  |  |__datatype.reds                      226
|  |  |__context.reds                       538
|  |  |__common.reds                        765
|  |  |__char.reds                          267
|  |  |__block.reds                        1599
|  |  |__bitset.reds                        901
|  |  |__binary.reds                       1131
|  |  |__action.reds                        181
|  |
|  |__+runtime/platform/
|  |  |__win32.reds                         472
|  |  |__win32-gui.reds                     108
|  |  |__win32-cli.reds                     270
|  |  |__win32-ansi.reds                    381
|  |  |__syllable.reds                       72
|  |  |__POSIX.reds                         451
|  |  |__linux.reds                          98
|  |  |__image-quartz.reds                  661
|  |  |__image-gdiplus.reds                 587
|  |  |__freebsd.reds                        79
|  |  |__darwin.reds                         82
|  |  |__COM.reds                           318
|  |  |__android.reds                       123
|
|__+system/
|  |__rsc.r                                 146
|  |__loader.r                              466
|  |__linker.r                              240
|  |__emitter.r                             715
|  |__config.r                              145
|  |__compiler.r                           3595
|  |
|  |__+system/bridges/
|  |  |
|  |  |__+system/bridges/java/
|  |  |  |__README.md                        18
|  |  |  |__JNIdemo.reds                     65
|  |  |  |__JNIdemo.java                     19
|  |  |  |__JNI.reds                        347
|  |
|  |__+system/formats/
|  |  |__PE.r                              1062
|  |  |__Mach-O.r                           852
|  |  |__Mach-APP.r                          71
|  |  |__Intel-HEX.r                        182
|  |  |__ELF.r                             1014
|  |
|  |__+system/library/
|  |  |__readme.txt                           2
|  |
|  |__+system/runtime/
|  |  |__win32.reds                         230
|  |  |__win32-driver.reds                   24
|  |  |__utils.reds                         158
|  |  |__system.reds                        129
|  |  |__syllable.reds                       49
|  |  |__start.reds                         105
|  |  |__POSIX.reds                         129
|  |  |__POSIX-signals.reds                  43
|  |  |__linux.reds                          40
|  |  |__linux-sigaction.reds               144
|  |  |__libc.reds                          190
|  |  |__lib-natives.reds                   122
|  |  |__lib-names.reds                      37
|  |  |__freebsd.reds                        91
|  |  |__debug.reds                         274
|  |  |__darwin.reds                        113
|  |  |__common.reds                        240
|  |  |__android.reds                        13
|  |
|  |__+system/targets/
|  |  |__target-class.r                     241
|  |  |__IA-32.r                           2086
|  |  |__ARM.r                             2499
|  |
|  |__+system/utils/
|  |  |__virtual-struct.r                   105
|  |  |__unicode.r                           34
|  |  |__secure-clean-path.r                 50
|  |  |__r2-forward.r                       125
|  |  |__profiler.r                         217
|  |  |__libRedRT.r                         272
|  |  |__libRedRT-exports.r                 317
|  |  |__int-to-bin.r                        20
|  |  |__IEEE-754.r                          91
|  |  |__encap-fs.r                          89
|
|__+utils/
|  |__redbin.r                              412
|  |__preprocessor.r                        333
|  |__extractor.r                            43
|  |__call.r                                319
```