# 电流传感器
在实际电路中需要实时获得电路的各种数据并反馈至处理器进行决策或供人为检测，以实现对电路可靠性提升和各种保护。相较于电压信号，电流信号更加难以直接检测，通常利用电流压降和电流的磁效应来实现电流检测。  
并联电阻和霍尔效应各有优缺点：
* 从原理上讲，并联电阻法是通过在电流路径上放置小阻值电阻，通过测量并联电阻上的微小压降，与精密放大器配合实现测量流经电阻的电流；而霍尔传感器则是利用霍尔效应，通过在导体两侧加入磁场，当电流流过时将在另外一侧产生电压，通过对电压的测量实现对电流的间接测量。
* 从精度上讲并联电阻传感器相较于霍尔传感器具有更高精度。  

具体对比如下图所示：
![方案对比1](https://github.com/HUSTdzc/Current_Sensing/blob/main/IMAGE/Screenshot%202024-05-08%20160746.png)  
![方案对比2](https://github.com/HUSTdzc/Current_Sensing/blob/main/IMAGE/ShuntVHall_Together-600x1080.jpg)  
影响精度的主要有温度变化、暂态大电流、传感器位置、辅助电源稳定性等相关。温度变化将会影响影响铁磁线圈气隙从而影响磁场强度，对于低温度系数电阻和低温漂放大器而言温度对并联电阻影响很小；暂态大电流可能会导致铁芯偏磁，在零点处仍有剩磁从而造成测量误差；霍尔传感器对安装位置有较高要求，若霍尔传感器靠近磁性元件或大功率开关回路将会受到磁场干扰；霍尔元件内部磁场朝向以及电压测量触点大小也会影响电流测量结果；此外辅助电源的稳定性对霍尔元件也有很大影响。  

在小电流低功率方面，动态响应和带宽方面二者近似，但是并联电阻法在大电流场合受到限制，过大的电流会在电阻上消耗过多功率，这对电阻的功率和精度要求较高；此外霍尔效应内部实现了电源隔离。

在高频环境下由于趋肤效应和临近效应，并联电阻法的测量效果与直流产生差异，高频测量通常采用同轴电阻法，两个通有反向电流的同轴电阻。但是同轴电阻体积过大难以集成。
除此之外还有电流互感器、罗氏线圈、Giant Magnetoresistive（GMR）巨磁阻电流传感器、磁阻电流传感器等等^[1]^，其对比如下图所示：
![电流测量方案](https://github.com/HUSTdzc/Current_Sensing/blob/main/IMAGE/Screenshot%202024-05-12%20213919.png)  
其中罗氏线圈的集成度如此之高是笔者未曾想象的，罗氏线圈由于其利用空心线圈避免了铁芯的引入，从而实现偏磁和迟滞饱和现象，达到了较高的线性度。此外罗氏线圈还具有测量大电流、带宽高、电气绝缘、暂态性能好等特点。下图介绍了集成罗氏线圈结构和实际电路尺寸：可以看到利用四层电路板，在上下两层通过反向电流从而形成磁场，再在中间两层利用PCB导线和过孔形成简易线圈，再通过集成运算放大器实现积分电路完成电流测量。最终电路尺寸和硬币近似，但是罗氏线圈具有无法测量直流电流的致命缺点，导致其仅适用于交流测量。
![罗氏线圈结构图](https://github.com/HUSTdzc/Current_Sensing/blob/main/IMAGE/Screenshot%202024-05-12%20221035.png)  
![罗氏线圈实际图](https://github.com/HUSTdzc/Current_Sensing/blob/main/IMAGE/Screenshot%202024-05-12%20221051.png)  

总之，并联电阻可以达到较高精度和较好的稳定性；而霍尔传感器易受外界干扰，但是功耗较低。在此笔者选用并联电阻法实现电流检测。

并联电阻有多种结构，可通过放大器直接将并联电阻两端电压放大作为模拟输出，也可后接ADC采样将模拟信号转化为数字信号，或者通过比较器输出逻辑变量用于过流保护检测。此外传感器还可接在多种位置，当接在高压侧如图所示：
![高压侧电流检测](https://github.com/HUSTdzc/Current_Sensing/blob/main/IMAGE/Screenshot%202024-05-09%20190108.png)  
高压侧接法可以检测负载对地短路电流，不受地点位波动影响；但是大部分场合其具有较高共模输入，需要放大器实现较高的共模抑制性。  
低压侧接法如下图所示：
![低压侧电流检测](https://github.com/HUSTdzc/Current_Sensing/blob/main/IMAGE/Screenshot%202024-05-09%20190126.png)  
低压侧实现简单，检测范围较宽，较低的共模偏置电压使得其功耗更小；但是无法检测负载对地漏电流，且系统地电位将会受到并联电阻影响。  

[1] Chucheng Xiao, Lingyin Zhao, T. Asada, W. G. Odendaal and J. D. van Wyk, "An overview of integratable current sensor technologies," 38th IAS Annual Meeting on Conference Record of the Industry Applications Conference, 2003., Salt Lake City, UT, USA, 2003, pp. 1251-1258 vol.2, doi: 10.1109/IAS.2003.1257710. 