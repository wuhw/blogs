#viewport的研究

1.在使用mediaquery时，如果通过min-width等width相关的进行查询时，其对比的是设备的viewport的width。手持设备在不设置viewport的时候，其宽度一般是980px，当设置成device-width时，其宽度是根据屏幕分辨率，屏幕像素密度，然后根据约定的屏幕像素密度对应的系数比例，计算出device-device.例如，iphone4s，屏幕分辨率是960*640，其屏幕像素密度是326ppi,对应的系数是2，所以device-width是640/2=320。

2.viewport的initial-scale的猜想，假如width=320,initial-scale=0.5,那么其含义是，缩小为原来的一般之后，viewport的width为320px。其提供给开发者使用的画布其实是640px。这样的好处是，当retina屏，@dpr(device-pixel-retio)=2，分辨率是960*640，视觉搞给的也是640时，我们就可以完全按照视觉搞得尺寸来设置。而不需要说/2之后再使用。