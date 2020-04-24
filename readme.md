//@version=4
//DepthHouse Indicators by oh92
//if you edit, please drop a line. always give credit where it is deserved :)
study("Relative Volume RVOL [DepthHouse]", overlay=false)

/// Inputs ///
l = input(26, title="Period")
p = input(1.75, title="Threshold")
s = input(14, title="Smoothing Value")
b = input(title="Volume Breakout based on:", options=["Smoothing Value", "Threshold"], defval="Smoothing Value")

/// Relative Volume Calcuations ///
vol = volume
svol = sma(vol, l)
rvol = vol/svol
srvol = sma(rvol, s)

/// Vol Breakout Condition ///
v_condition = b=="Smoothing Value" ? srvol : p
v_breakout = crossover(rvol,v_condition)

/// oh92 Favorite Colors ///
red   = #ef9a9a     // #ff848a // #FA8072 // #323433 // #ff848a // #FF510D 
green = #95f33b     // #8cffe5 // #6DC066 // #80aebd // #8cffe5 // #5AA650
light = #b2b5be
dark  = #787b86

/// Horizontal Lines ///
hline(0, title="Zero Line")
hline(p, title="Threshold")

/// Plots & Color Calcuations///
plot(rvol, color=rvol>rvol[1]?green:red, style=plot.style_histogram, linewidth=2, transp=0, title="Relative Vol")
//plot(srvol>rvol ? rvol/2 : srvol, color=close>close[1]?green:red, style=plot.style_histogram, linewidth=2, transp=10, title="+/- Vol")
plot(srvol, color=dark, transp=5, title="Smoothed RVol")

/// Breakout Plotshape ///
plotshape(v_breakout, location=location.top, style=shape.diamond, color=close>close[1]?green:red, size=size.auto, transp=0, offset=0, title="Volume Breakout")

/// Alert Conditions ///
alertcondition(crossover(rvol,srvol), title="Smoothed Vol Breakout", message="{{ticker}}:Volume Breakout based on Smoothed Vol")
alertcondition(crossover(rvol,p), title="Threshold Vol Breakout", message="{{ticker}}:Volume Breakout based on Threshold")
