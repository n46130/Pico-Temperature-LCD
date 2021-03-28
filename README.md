# Pico-Temperature-LCD

#Temperature_adc.py
from math import ceil
import machine
import utime
temperature = machine.ADC(26)
led = machine.Pin(15, machine.Pin.OUT)


print ("HELLO TEMP_ADC")
led.value(0)

while True:
    conversion_factor = 3.3/(65535)
    #get_absolute_time()
   
    x = (temperature.read_u16()) * conversion_factor
    x2 = round(x,3)
   
    print ('x2:  ', x2)
    if x >= .73:
        led.value(1)
        utime.sleep(10)
        print ("LED")
        led.value(0)
       
    tempC = (x-.520)*100    
    tem = round(((tempC*9/5) +32),2)
    tempF = ceil(tem)
   
    print ('%9.3f'%x2,"   ",  "TEMP C  ", '%9.3f'%round(tempC,3),"   ", "TEMP F", '%9.3f'%round(tempF,2))
   
    sda = machine.Pin(0)
    scl = machine.Pin(1)
    i2c = machine.I2C(0, sda = sda, scl = scl, freq = 400000)
    print(i2c.scan())  #Prints 114

    utime.sleep_ms(100)

    i2c.writeto(114, '\x7C')   #Sending Characters in HEX

    i2c.writeto(114, '\x2D')   #Clear display and set cursor to start position
    utime.sleep_ms(100)                                                                                                                                                                                                                                                                                                                                                                                                                        
    print(tempF)
    #i2c.writeto(114,"TEMP F: ", + str(tempF))
    i2c.writeto(114,"TEMP " + str(tempF ) + chr(223)+"F")  
    utime.sleep(2)
   




