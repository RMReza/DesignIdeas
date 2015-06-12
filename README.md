# DesignIdeas
This is a repository to learn GitHub and share some ideas, etc.

Hi guys, 
This is my first try so there may be some faults.

Here are some points about the adding ADC to TI SensorTag.
TI SensorTag uses CC2650 as a microcontroller.
TI DevPack uses Tiva TM4C1294NCPDT Cortx M4F series for debugging and programming of the CC2650. It also provides sensor expansop CONNECTORs for SensorTag. So, sensors are processed by CC2650 not Tiva TM4C1294NCPDT.
You can add sensor to TI SensorTag using TI DevPack in one of the following methods:
1. Grove I2C
            1.1 AD7997: To use AD7997, you have to configure the following pins
                            As: Logic input used for addressing for initilizing of the ADC's registers. It must be generate                                  by the CC2560 (Tables 6)
                            CONVST: Edge tiggered logic input that tells the ADC to initiate conversion. Precise timimg is                                       required . CC2650 also has to provide this signal. Without it you wouldn't know what to                                      do.
                            ALERT/Busy: output logic singal determining the state of the ADC. Alert means, there is a big in                                     process, and busy means that ADC is doing conversion. CC2650 has to track it.
                            SDA: Standard I2C bidirectional digital data path. CC2560 has to read and wirte on this pin 
                            SCL: Serial clock for I2C. CC2560 has to generate.
                        
                         Summary: You have to used Grove I2C for SDA and SCL.
                                  In addition to Grove I2C you have to use two Grove1 A/D and Grove2 A/D and  connectors for                                   As, CONVST, and ALERT/Busy.
                                  The pin congiuration between CC2560 and TI DevPack is as following:
                                      P4 Grove I2C: SDA ---> DIO5
                                                    SCL ---> DIO6
                                      P3 Grove1 A/D:
                                                    DP0 ---> DIO25 e.g can be used for As.  
                                                    DP1 ---> DIO24 e.g can be used for CONVST.
                                      P5 Grove1 A/D:
                                                    DP2 ---> DIO23 e.g can be used for ALERT/BUSY. 
                                                    DP3 ---> DIO27
                                  You also need to connect VDD of ADC to XDSET_VCC (P2.2) and AGND to GND (P2.1)
                                  Also, reference voltage is required for Ref pin.
            1.2 ADS7828: This is much easier than AD7997
