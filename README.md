중간고사 – 202001673 조민기
=============

**고속 푸리에 변환(Fast Fourier Transform, FFT)이란?**<br/>
: 이산 푸리에 변환(Discrete Fourier Transform, DFT)과 역변환을 고속으로 수행하는 알고리즘.<br/>
* DFT: 이산적인, 즉 불연속적인 입력 신호에 대한 푸리에 변환(Fourier Transform).<br/>
* FT: 시간이나 공간에 대한 함수를 시간 또는 공간 주파수 성분으로 분해하는 변환.<br/>
<br/>
시간이나 공간에 대한 어떤 함수, 즉 푸리에 변환의 ‘입력’에 해당하는 함수는 다양한 주파수를 갖는 많은 함수들의 합으로 구성되어 있다. 푸리에 변환을 거친 함수는 특정 주파수 영역에서만 값이 급등하게 되는데, 이를 통해 ‘입력’을 구성하고 있는 다양한 주파수들을 확인할 수 있다.<br/>
<br/>
예를 들어, 1초에 2번 진동하는 주파수를 갖는 함수 G와 1초에 3번 진동하는 주파수를 갖는 함수 H가 합쳐진 함수 F를 푸리에 변환하게 되면, 변환한 결과 F는 주파수 2, 3에서 값이 급등할 것이다.<br/>
이를 통해 F는 함수 G와 H로 구성되어 있음을 확인할 수 있다.<br/>
<br/>
FT의 역, 즉 역변환은 이러한 FT의 결과를 기존의 함수로 역변환하는 것을 의미한다.<br/>
<br/>
이러한 FT의 입력 신호, 즉 함수가 불연속적일 때 하는 FT가 바로 DFT이고, DFT와 그의 역변환을 고속으로 수행하는 알고리즘이 바로 FFT이다.<br/>
<br/>
***
<br/>
