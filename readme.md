yz_keyboard
========

diy机械键盘，类似HHKB的布局，修改fn的位置，修改功能键定义，并加入鼠标功能。
参考：http://tieba.baidu.com/p/4592747695  
可以考虑加入的功能：  
1. 录制宏
2. 通过统计按键风格做一些智能动作,例如操作者识别，操作者工作状态、工作类别统计  

#布局
使用键位编辑网站Keyboard-layout-editor编辑键盘的布局：http://www.keyboard-layout-editor.com/  
使用定位板生成网站http://builder.swillkb.com/生成dxf  
将生成的文件导入Altium designer后，作为机械层辅助布局  

仿HHKB的设计，使用59键，通过fn键复用按键。将fn位置放在左手大拇指处  
复用按键：(失去所有复用按键的组合功能)  
1. F1~F12: 通过第一排按键实现  
2. del: 通过backspace和~实现  
3. 方向键： 通过vim风格的hjkl键实现  
4. home、end、pgup、pgdn：fn+a+hjkl(失去a+上下左右功能)  
5. insert、prtscren：fn+i,fn+p  
6. 上一级（alt+up）: fn+u  
7. 鼠标上下左右滚轮：fn+s+hjklnm(失去s+上下左右功能)  
8. 鼠标大幅上下左右：fn+a+s+hjkl  
9. 鼠标按键：fn+dfg：左键中键右键  
10. 鼠标拖动：fn+dfg+hjklnm  
11. 鼠标大幅拖动：fn+a+dfg+hjklnm  

#实现
为了实现全键任意组合，单片机的每一个引脚连接一个按键，单独进行扫描。  
通过传感引脚控制led的亮度，led设在按键上拉电阻后，成为上拉回路中的一部分。按键按下时，led最大亮度。按键松开时，单片机控制引脚为输出模式，低电平亮灯；设置为输入上拉模式时，灯灭。通过占空比实现亮度调节。  
扫描和灯的控制共用0.5ms的时隙，10ms为一组，每个时间组首先扫描按键，扫描完成后按照灯的亮度设定设置引脚是否需要输出。10ms后重新设置为输入模式扫描按键。所以按键扫描频率为100Hz，led的PWM频率为100Hz，占空比为20档。

