# 数据精度

通过Edge接入到EnOS™平台中的数据精度由两部分采样频率决定：

1. Edge与设备之间的采样频率

   该采样频率由通讯规约中配置的采样频率决定，这部分采样频率决定了Edge采集到的数据的原始精度。

2. Edge与云端之间的传输频率

   该采样频率在Edge中的设备接入模板上进行定义，有3种数据上传模式：

   - 实时上送（realtime），Edge会根据规约层定义的采样频率，采集到数据后直接转送到云端，规约层的每一次数据采集刷新都会向云端上送数据，最终上送到云端的精度与规约层的采集精度一致。

   - 定时上送（timer），即根据设备模型中定义的上传频率，数据从Edge中周期性的上传到云端，如定义定时上送的时间为10秒，则Edge会对采集到的数据进行切割处理，每10秒向云端上送一次数据，注意如果在10秒的周期内Edge侧没有收到过该值，则不会上送；

   - 变化上送（change），即在Edge中会校验采集到的数据，如果数值发生变化就上送到云端，该模式主要应用于DI类型数据。

最终存储到EnOS™平台中的数据分辨率是在步骤2）中定义的，但数据的实际精度取决于这两个步骤共同作用。例如在步骤2）中即使配置了高精度的数据上送频率，但由于规约层的采集频率较低，则最终存储到EnOS™平台中的也只是重复数据而已。
