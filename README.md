# Red-Scalper

// This source code is subject to the terms of the Mozilla Public License 2.0 at https://mozilla.org/MPL/2.0/
//
// @version=5
//
// TMO (T)rue (M)omentum (O)scillator) MTF Special Scapler Version
//
// TMO Scalper is a special custom version that was designed exclusively for lower time frame scalps (1-5min charts)
// 
// This version of the TMO Oscillator print the signals ONLY when the higher aggregation is aligned with the current (lower aggregation)
//
// Created by L&L Capital
// 

indicator("TMO Scalper", shorttitle="TMO Scalper", overlay=false) 

// Inputs

tmolength = input.int(14,title="Length")
calcLength = input(5, title="Calc Length")
smoothLength = input(3, title="Smooth Length")
dotsize = input(3, title="Signal Size")
offset = input(2,title="Signal Offset")

// TMO 1 Calculations

TimeFrame1 = input.timeframe('1', "TMO 1", options=['1','2','3','5','10','15','20','30','45',"60","120","180","240",'D','2D','3D','4D','W','2W','3W','M','2M','3M'])

srcc1 = request.security(syminfo.tickerid, TimeFrame1, close)
srco1 = request.security(syminfo.tickerid, TimeFrame1, open)
o1 =  srco1
c1 =  srcc1

data1 = 0
for i1 = 1 to tmolength -1
    if c1 > o1[i1]
        data1 := data1 + 1
    if c1 < o1[i1]
        data1 := data1 - 1


EMA1 = ta.ema(data1,calcLength)
Main1 = request.security(syminfo.tickerid, TimeFrame1, ta.ema(EMA1, smoothLength))
Signal1 = request.security(syminfo.tickerid, TimeFrame1, ta.ema(Main1, smoothLength))

// TMO 2 Calculations

TimeFrame2 = input.timeframe('5', "TMO 2", options=['1','2','3','5','10','15','20','30','45',"60","120","180","240",'D','2D','3D','4D','W','2W','3W','M','2M','3M'])

srcc2 = request.security(syminfo.tickerid, TimeFrame2, close)
srco2 = request.security(syminfo.tickerid, TimeFrame2, open)

o2 =  srco2
c2 =  srcc2

data2 = 0
for i2 = 1 to tmolength -1
    if c2 > o2[i2]
        data2 := data2 + 1
    if c2 < o2[i2]
        data2 := data2 - 1


EMA2 = ta.ema(data2,calcLength)
Main2 = request.security(syminfo.tickerid, TimeFrame2, ta.ema(EMA2, smoothLength))
Signal2 = request.security(syminfo.tickerid, TimeFrame2, ta.ema(Main2, smoothLength))

// TMO 3 Calculations

TimeFrame3 = input.timeframe('30', "TMO 3", options=['1','2','3','5','10','15','20','30','45',"60","120","180","240",'D','2D','3D','4D','W','2W','3W','M','2M','3M'])

srcc3 = request.security(syminfo.tickerid, TimeFrame3, close)
srco3 = request.security(syminfo.tickerid, TimeFrame3, open)

o3 =  srco3
c3 =  srcc3

data3 = 0
for i3 = 1 to tmolength -1
    if c3 > o3[i3]
        data3 := data3 + 1
    if c3 < o3[i3]
        data3 := data3 - 1


EMA3 = ta.ema(data3,calcLength)
Main3 = request.security(syminfo.tickerid, TimeFrame3, ta.ema(EMA3, smoothLength))
Signal3 = request.security(syminfo.tickerid, TimeFrame3, ta.ema(Main3, smoothLength))

// TMO Scalper Signals

mainln = (15*Main1/tmolength)
signaln = (15*Signal1/tmolength)
mainln2 = (15*Main2/tmolength)
signaln2 = (15*Signal2/tmolength)
mainln3 = (15*Main3/tmolength)
signaln3 = (15*Signal3/tmolength)

// TMO 1 Bullish Signal    

plotchar(ta.crossover(mainln, signaln) and (mainln2 > signaln2) ? (mainln-offset) : na, title = "TMO 1 Bullish Signal", char = '▴', location = location.absolute, size = size.small, color = #0de50d)

// TMO 1 Bearish Signal
plotchar(ta.crossover(signaln, mainln) and (mainln2 < signaln2) ? (mainln+offset) : na, title = "TMO 1 Bearish Signal", char = '▾', location = location.absolute, size = size.small, color = #ff0000)

//TMO 2 Bullish Signal
plotchar(ta.crossover(mainln2, signaln2) and (mainln3 > signaln3) ? (mainln2-offset) : na, title = "TMO 2 Bullish Signal", char = '▴', location = location.absolute, size = size.normal, color = #006400, display = display.none)

//TMO 2 Bearish Signal
plotchar(ta.crossover(signaln2, mainln2) and (mainln3 < signaln3) ? (mainln2+offset) : na, title = "TMO 2 Bearish Signal", char = '▾', location = location.absolute, size = size.normal, color = #800000, display = display.none)

// TMO 1 Plots

color1 = Main1 > Signal1 ? #27c22e : #ff0000
mainLine1 = plot(15*Main1/tmolength, title="TMO 1 Main", color=color1)
signalLine1 = plot(15*Signal1/tmolength, title="TMO 1 Signal", color=color1)
fill(mainLine1, signalLine1, title="TMO 1 Fill", color=color1, transp=75)

// TMO 2 Plots

color2 = Main2 > Signal2 ? #27c22e : #ff0000
mainLine2 = plot(15*Main2/tmolength, title="TMO 2 Main", color=color2)
signalLine2 = plot(15*Signal2/tmolength, title="TMO 2 Signal", color=color2)
fill(mainLine2, signalLine2, title="TMO 2 Fill", color=color2, transp=75)

// TMO 3 Plots

color3 = Main3 > Signal3 ? #006400 : #800000
mainLine3 = plot(15*Main3/tmolength, title="TMO 3 Main", color=color3)
signalLine3 = plot(15*Signal3/tmolength, title="TMO 3 Signal", color=color3)
fill(mainLine3, signalLine3, title="TMO 3 Fill", color=color3, transp=75)

// Background & Colors

upper = hline(10, title="OB Line", color=#ff0000, linestyle=hline.style_solid,display=display.none)
lower = hline(-10, title="OS Line", color=#27c22e, linestyle=hline.style_solid,display=display.none)
ob = hline(math.round(15), title="OB Cutoff Line", color=#ff0000, linestyle=hline.style_solid)
os = hline(-math.round(15), title="OS Cutoff Line", color=#27c22e, linestyle=hline.style_solid)
zero = hline(0, title="Zero Line", color=#2a2e39, linestyle=hline.style_solid)
fill(ob, upper, title="OB Fill", color= #ff0000, transp = 75)
fill(os, lower, title="OS Fill", color= #27c22e, transp = 75)
