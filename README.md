# bat批处理猜拳小游戏

#### 介绍
用bat批处理实现的一个猜拳小游戏，
无意间了解了bat批处理的强大之处，非常感兴趣，便借助实现一个人机猜拳小游戏，熟练以下bat语法

#### 软件架构
在桌面新建一个 1.bat 文件，然后将以下代码复制进去，保存关闭，双击运行即可

``` bat
@echo off

:: ============ 定义变量 ==============
:: 当前局数、胜利数、平局数、失败数
set ci=0
set sheng=0
set ping=0
set bai=0
:: 等级
set lv=0

call:play

pause
exit

::开始
:play
echo * * * * * * * * * *  欢迎来到--剪子包袱锤  * * * * * * * * * *
echo;
echo;
echo 1、开始游戏 - 白痴
echo 2、开始游戏 - 正常
echo 3、开始游戏 - 地狱
echo 4、查看战绩
echo 5、退出
echo;
echo;
choice /c 12345
set n=%ERRORLEVEL%
if %n% equ 1 set lv=1 & call:run
if %n% equ 2 set lv=2 & call:run
if %n% equ 3 set lv=3 & call:run
if %n% equ 4 call:zj
if %n% equ 5 call:zj & pause & exit
goto play

goto:eof


:: 输出战绩
:zj
echo;
echo -----战绩----- 总次数： %ci%   胜利：%sheng%    失败： %bai%   和局： %ping%
echo;
goto:eof


:: 进入对局模式
:run

rem 死循环段开始
:for2
call:onepk
if %ruc% equ 4 goto:eof
goto:for2
rem 死循环段结束

goto:eof


:: 进行一次对局
:onepk

set /a ci=%ci%+1
echo;
echo;
echo ========================= 第 %ci% 局 ========================
call:uc
set uc=%return%
set ruc=%uc%
if %ruc% equ 4 goto:eof

if %lv% equ 1 call:dc1 %uc%
if %lv% equ 2 call:dc2
if %lv% equ 3 call:dc3 %uc%
set dc=%return%

call:getstr %uc%
echo 你的出拳----%return%
call:getstr %dc%
echo 电脑的出拳----%return%

echo;
choice /t 1 /d y /n >nul
call:pk %uc% %dc%
if %return% equ 1 set /a sheng=%sheng%+1 & echo ====================== 你胜了
if %return% equ 2 set /a ping=%ping%+1 & echo ====================== 平局 
if %return% equ 3 set /a bai=%bai%+1 & echo ====================== 你败了
call:zj


goto:eof


:: 返回出拳字符串
:getstr
if %1 equ 1 set return=石头
if %1 equ 2 set return=剪刀
if %1 equ 3 set return=布
goto:eof

:: 返回pk结果, 
:: 参数 uc dc
:: 返回 1=胜利，2=平局，3=失败
:pk 
if %1 equ 1 if %2 equ 2 set return=1
if %1 equ 2 if %2 equ 3 set return=1
if %1 equ 3 if %2 equ 1 set return=1
if %1 equ %2 set return=2
if %1 equ 1 if %2 equ 3 set return=3
if %1 equ 2 if %2 equ 1 set return=3
if %1 equ 3 if %2 equ 2 set return=3
goto:eof


::返回一次用户出拳
:uc
echo 请出拳：S=石头，J=剪刀，B=布，T=返回菜单
choice /c sjbt
set return=%ERRORLEVEL%
goto:eof


:: 返回一次电脑出拳，必败
:dc1
if %1 equ 1 set return=2
if %1 equ 2 set return=3
if %1 equ 3 set return=1
goto:eof

:: 返回一次电脑出拳，随机
:dc2
call:random 1 3
set retutn=%return%
goto:eof

:: 返回一次电脑出拳，必胜
:dc3
if %1 equ 1 set return=3
if %1 equ 2 set return=1
if %1 equ 3 set return=2
goto:eof

:: 返回随机数
:random
set min=%1
set max=%2
set /a num=%RANDOM%%%(%max%+1-%min%)+%min%
set return=%num%
goto:eof
```

