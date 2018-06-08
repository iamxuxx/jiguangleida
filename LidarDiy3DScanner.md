# DIY激光雷达3D测量扫描仪介绍
用上海思嵐科技的RPLIDAR—A1型号，DIY了一个3D测量扫描仪。RPLIDAR—A1是基于三角形测量原理的传感器。具体见  
<http://www.slamtec.com/cn/lidar/A3>  
该设计是做平面测量扫描的。

# 设计成3D测量扫描
本来RPLIDAR自己带有一个旋转运动，只做平面测量扫描，即2D的。  
为了实现3D测量扫描，需要增加一个运动：  
1、平移运动  
2、旋转运动    
3、平移加旋转运动  
考虑容易入手的理由，选择2。  

# DIY原型机照片
![alt](images/Scan3D.jpe
g)

# diy设计的元器件构成
|No.| 名称      | 数量 | 备注 |
|---| ------------- |---------------|---------------|
|1|RPLIDAR本机 |1 | 即传感器|
|2|USB-UART模块 |1 | RPLIDAR套装|
|3|UNO|1 | 单片机|
|4|步进马达|1 | |
|5|光电开关|1 | |
|6|杜邦线|9|公母头 |
|7|三脚架|1 | |
|8|电脑|1 | |
|9|USB1|1|安卓手机小口 |
|10|USB2|1|方口 |
|11|机械零部件|3|3D打印 |
|12|螺丝螺母|4|M2.5 |


# 采样数据视频示例
<https://www.youtube.com/watch?v=YGQJd-3JXq0>

# UNO代码
｀｀｀｀  
u8 i,j;
  for(j=0;j<4;j++){
     for(i=0;i<4;i++)t[i]=s[i];
     t[j]=HIGH;
     for(i=0;i<4;i++)digitalWrite(p[i], t[i]); 
     delay(5);
   } 
}
void ccw(void){
  u8 i,j;
  for(j=0;j<4;j++){
     for(i=0;i<4;i++)t[i]=s[i];
     t[3-j]=HIGH;
     for(i=0;i<4;i++)digitalWrite(p[i], t[i]); 
     delay(5);
     flg=0x99;   
     Serial.write(flg); 
   } 
}
void org(){while(c==0){
  c=digitalRead(speedPin);cw();delay(5);}ccw();
}

void rset()
{
    c=digitalRead(speedPin);
    if(c==0)org();
     else
     {
      while(c==1){ c=digitalRead(speedPin);ccw();delay(20);}
      org();flg=0xaa;
 //     Serial.write(flg);
     }
}

void loop() {
  delay(50);
  switch(flg)
  {
    case 0x55:rset();flg=0;break; //reset 55
    case 0x56:cw();c=digitalRead(speedPin);Serial.write(c);break;//clockwise 56
    case 0x57:ccw();c=digitalRead(speedPin);Serial.write(c);break;//counterclockwise 57
    case 0x58:Stop(); break; //stop
    case 0x5a:c=digitalRead(speedPin);cw();Serial.write(c);flg=0;delay(50);break;//step cw
    default:break; 
  }

}

void serialEvent() {
  while (Serial.available()) {
    inChar = (char)Serial.read();
    flg=inChar;
 //   Serial.write(flg);
    return;
  }
}
｀｀｀｀  

＃ PC代码
｀｀｀｀

｀｀｀｀


``` 
u8 i,j;
  for(j=0;j<4;j++){
     for(i=0;i<4;i++)t[i]=s[i];
     t[j]=HIGH;
     for(i=0;i<4;i++)digitalWrite(p[i], t[i]); 
     delay(5);
   } 
}
void ccw(void){
  u8 i,j;
  for(j=0;j<4;j++){
     for(i=0;i<4;i++)t[i]=s[i];
     t[3-j]=HIGH;
     for(i=0;i<4;i++)digitalWrite(p[i], t[i]); 
     delay(5);
     flg=0x99;   
     Serial.write(flg); 
   } 
}
void org(){while(c==0){
  c=digitalRead(speedPin);cw();delay(5);}ccw();
}

void rset()
{
    c=digitalRead(speedPin);
    if(c==0)org();
     else
     {
      while(c==1){ c=digitalRead(speedPin);ccw();delay(20);}
      org();flg=0xaa;
 //     Serial.write(flg);
     }
}

void loop() {
  delay(50);
  switch(flg)
  {
    case 0x55:rset();flg=0;break; //reset 55
    case 0x56:cw();c=digitalRead(speedPin);Serial.write(c);break;//clockwise 56
    case 0x57:ccw();c=digitalRead(speedPin);Serial.write(c);break;//counterclockwise 57
    case 0x58:Stop(); break; //stop
    case 0x5a:c=digitalRead(speedPin);cw();Serial.write(c);flg=0;delay(50);break;//step cw
    default:break; 
  }

}

void serialEvent() {
  while (Serial.available()) {
    inChar = (char)Serial.read();
    flg=inChar;
 //   Serial.write(flg);
    return;
  }
}
```

