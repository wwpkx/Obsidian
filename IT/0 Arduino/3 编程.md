# 常用函数
```
数字 I/O 
pinMode(pin, mode)
digitalWrite(pin, value)
digitalRead(pin)

模拟 I/O 
analogReference(type)
analogRead(pin)
analogWrite(pin, value) - PWM 

高级 I/O 
shiftOut(dataPin, clockPin, bitOrder, val)
pulseIn(pin, state, timeout)

时间
millis()
delay(ms)
delayMicroseconds(us)

随机数
randomSeed(seed)
random(howbig)
random(howsmall, howbig)

位操作
lowByte()
highByte()
bitRead()
bitWrite()
bitSet()
bitClear()
bit()
```