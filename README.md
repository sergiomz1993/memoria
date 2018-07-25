//// memoria eeprom 24c512 para mudulo rfid
                                                                                                                                                                          
// Read from I2C slave at address 0x62
#include "mbed.h"
#define eepr_addr 0x50

I2C i2c(p9, p10); //sda   scl
DigitalOut led(LED1);
Serial pc(USBTX,USBRX);

int main() {
    i2c.frequency(400000);
    uint8_t tx_data[5]="HOLA";
    uint8_t rx_data[1];
    
    //secuencia de escritura
    i2c.start();
    int ack=i2c.write(eepr_addr<<1);
    
    if(ack==1){
        i2c.write(0x00);
        i2c.write(0x01);
        int i=0;
        while(tx_data[i]!=NULL){
            i2c.write(tx_data[i]);
            i++;
            }
        i2c.stop();
        pc.printf("Escritura exitosa\n");    
        }    
        else if(ack==0){
            pc.printf("NACK");
            }    
            else if(ack==2){
                pc.printf("TIMEOUT");
                }
    
    //secuencia de lectura
    
    i2c.start();
    ack=i2c.write(eepr_addr<<1);
    if(ack==1){
        i2c.write(0x00);
        i2c.write(0x02);
        i2c.start();
        i2c.write(eepr_addr<<1|0x01);
        rx_data[0]=i2c.read(0);
        i2c.stop();
        pc.printf("%c",rx_data[0]);
        }

while(1){
    led=0;
    wait_ms(500);
    led!=led;
    }    
    
}
  
  
