Этот сегмент памяти в [[heap]] в котором аллоцируются новые объекты. В этой области памяти объекты не живут больше одной сборки [[GC]]. 

Все выжившие элементы переезжают в [[Survival(S0 & S1)]] область сразу же после сборки мусора. Остальные удаляются.