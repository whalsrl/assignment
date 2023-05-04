중간고사 – 202001673 조민기
=============

**고속 푸리에 변환(Fast Fourier Transform, FFT)이란?**<br/>
: 이산 푸리에 변환(Discrete Fourier Transform, DFT)과 역변환을 고속으로 수행하는 알고리즘.<br/>
* DFT: 이산적인, 즉 불연속적인 입력 신호에 대한 푸리에 변환(Fourier Transform).<br/>
* FT: 시간이나 공간에 대한 함수를 시간 또는 공간 주파수 성분으로 분해하는 변환.<br/>
<br/>
시간이나 공간에 대한 어떤 함수, 즉 푸리에 변환의 ‘입력’에 해당하는 함수는 다양한 주파수를 갖는 많은 함수들의 합으로 구성되어 있다. 푸리에 변환을 거친 함수는 특정 주파수 영역에서만 값이 급등하게 되는데, 이를 통해 ‘입력’을 구성하고 있는 다양한 주파수들을 확인할 수 있다.<br/>
<br/>
예를 들어, 1초에 2번 진동하는 주파수를 갖는 함수 G와 1초에 3번 진동하는 주파수를 갖는 함수 H가 합쳐진 함수 F를 푸리에 변환하게 되면, 변환한 결과 F2는 주파수 2, 3에서 값이 급등할 것이다.<br/>
이를 통해 F는 함수 G와 H로 구성되어 있음을 확인할 수 있다.<br/>
<br/>
FT의 역, 즉 역변환은 이러한 FT의 결과를 기존의 함수로 역변환하는 것을 의미한다.<br/>
<br/>
이러한 FT의 입력 신호, 즉 함수가 불연속적일 때 하는 FT가 바로 DFT이고, DFT와 그의 역변환을 고속으로 수행하는 알고리즘이 바로 FFT이다.<br/>
<br/>
=============
<br/>
**DFT의 수학적 분석**<br/>
<br/>
푸리에 급수에 의하면, 주기 p를 갖는 어떤 함수는 주기가 p인 삼각함수들의 무한한 합과 같다.<br/>
(정확히는 sin 함수와 cos 함수들의 합)<br/>
<br/>
즉 기존의 직교좌표계보다는 극좌표계의 관점에서 바라보는 것이 훨씬 편리하다.<br/>
이 중 복소 평면의 경우 실수부가 cos 함수, 허수부가 sin 함수에 해당된다.<br/>
그러므로 sin, cos 함수만으로 구성된 푸리에 변환은 대부분 복소 평면에서 구현된다.<br/>
또한 sin, cos함수의 합은 오일러 공식을 통해 자연상수 e의 지수함수로 변환할 수 있다.<br/>
(이러한 오일러 공식은 맥클로린 급수를 통해 증명되지만, 본문에서는 생략)<br/>
<br/>
그리고 FFT는 DFT를 고속으로 구현하는 알고리즘이므로 DFT를 구현할 수 있어야 한다.<br/>
DFT는 이산적인 입력을 받으므로, 적분할 필요 없이 구해진 모든 값을 더하기만 하면 된다.<br/>
위 식은 다음과 같이 변형할 수 있다. (편의상 N=4인 상황을 예시로 들었음)<br/>
<br/>
위 식을 일반화하면 다음과 같다.<br/>
<br/>
짝수 인덱스의 항을 살펴보면, 짝수 인덱스끼리 DFT를 한 값과 동일하다는 사실을 알 수 있다.<br/>
또한 복소수 배열의 원래 크기인 N이 아닌, N/2에 대한 식으로 나타나 있음을 확인할 수 있다.<br/>
<br/>
그리고 k가 N의 절반보다 클 경우, 홀수 인덱스 항에서는 다음과 같은 현상이 일어난다.<br/>
<br/>
즉, 절반 이후의 홀수번째 항들은 절반 이전의 홀수번째 항에 –1을 곱한 값과 동일하다.<br/>
<br/>
=============
<br/>
**DFT의 특성**<br/>
<br/>
짝수 인덱스의 DFT를 a, 홀수 인덱스의 DFT를 b, b의 계수에 해당하는 지수함수를 c라고 할 때,<br/>
DFT를 일반화한 식을 통해 2가지의 특성을 확인할 수 있다.<br/>
<br/>
1. DFT는 a + (c*b)로 분리할 수 있으며, a와 b도 다시 2개의 DFT로 분리할 수 있다.
2. 절반 이전의 인덱스에서는 DFT가 a와 c*b의 합이지만,절반 이후의 인덱스에서는 DFT가 a에서 c*b를 뺀 값이 된다.
<br/>
=============
<br/>
**알고리즘 구현 과정**<br/>
<br/>
일반화한 식을 바탕으로 알고리즘을 구현하면  다음과 같다.<br/>
<br/>
1. 크기가 N인 복소수 배열을 매개변수로 받는다.
2. 크기가 N/2인 홀수 배열과 짝수 배열을 만들고,반복문으로 각 배열에 해당하는 인덱스의 원소를 대입한다.
3. 복소수 배열 a와 b를 만든다.a와 b에 각각 FFT(짝수), FFT(홀수)를 대입한다. <재귀 호출 구조>배열 a에는 짝수 배열의 FFT 결과가, b에는 홀수 배열의 FFT 결과가 담기게 된다.
4. 결과를 저장할 복소수 배열을 만든다.
5. k가 0에서 (N/2 – 1)이 될 때까지 반복하는 반복문을 만든다.
6. 반복문 안에 들어갈 내용은 다음과 같다.<br/>- 일반화한 식의 지수에 해당하는 부분을 만든다. (-2*PI*k/크기)<br/>- 해당하는 지수를 cos, sin 함수에 집어넣어 복소수 c를 만든다.<br/>- 결과의 k번째 인덱스에는 a[k]와 (b[k] * c)의 합이 들어온다.<br/>- 절반 이후, 즉 결과의 k + (N/2) 인덱스에는 반대로 a[k] - (b[k] * c)가 들어온다.
7. 결과 배열을 반환한다.
<br/>
자바의 경우 복소수 자료형을 지원하지 않으므로, 프로그래머 본인이 복소수 클래스를 만들어야 한다.<br/>
해당 클래스 안에는 복소수의 기본적인 특성인 실수부와 허수부의 구현, 복소수의 기본적인 사칙연산 등의 기능이 들어있어야 한다.<br/>
<br/>
위 알고리즘의 시간복잡도는 반복문의 O(N)과 재귀 호출의 O(log N)의 곱인 O(N log N)이므로, 기존 식을 토대로 세운 코드의 시간복잡도인 O(N^2)보다 빠르다. 즉 FFT라고 할 수 있다.<br/>
<br/>
(알고리즘을 토대로 구현한 코드의 정확도를 확인하기 위해, 기존의 DFT 또한 구현했습니다.)<br/>
<br/>
=============
<br/>
**코드 구현**<br/>
<br/>
```
import java.lang.*;
public class Main {
    public static Complex[] temp;

    public static void DFT(Complex c[]) {
        int N = c.length;

        for(int k = 0; k < N; k++) {
            for(int n = 0; n < N; n++) {
                double theta = (-2) * Math.PI * k * n / N;
                Complex sc = new Complex(Math.cos(theta), Math.sin(theta));
                temp[k] = temp[k].add(sc.mul(c[n]));
            }
        }
    }
    public static Complex[] FFT(Complex c[]) {
        int N = c.length;

        if(N == 1) {
            return new Complex[] {c[0]};
        }

        Complex even[] = new Complex[N / 2];
        Complex odd[] = new Complex[N / 2];

        for(int i = 0; i < N / 2; i++) {
            even[i] = c[2 * i];
            odd[i] = c[2 * i + 1];
        }

        Complex[] a = FFT(even);
        Complex[] b = FFT(odd);

        Complex[] result = new Complex[N];
        for(int k = 0; k < N / 2; k++) {
            double theta = (-2) * Math.PI * k / N;
            Complex sc = new Complex(Math.cos(theta), Math.sin(theta));
            result[k] = a[k].add(sc.mul(b[k]));
            result[k + N / 2] = a[k].sub(sc.mul(b[k]));
        }

        return result;
    }

    public static void printArray(Complex[] c) {
        for (int i = 0; i < c.length; i++)
            System.out.print(c[i] + " ");
        System.out.println();
    }

    public static void main(String[] args) {
        Complex[] src = new Complex[8];
        temp = new Complex[src.length];
        for(int i = 0; i < src.length; i++) {
            temp[i] = new Complex(0.0, 0.0);
        }

        src[0] = new Complex(1.0, 2.0);
        src[1] = new Complex(2.0, 1.0);
        src[2] = new Complex(3.0, 9.0);
        src[3] = new Complex(4.0, 6.0);
        src[4] = new Complex(5.0, 10.0);
        src[5] = new Complex(6.0, 1.0);
        src[6] = new Complex(7.0, 10.0);
        src[7] = new Complex(8.0, 4.0);

        printArray(src);
        DFT(src);
        printArray(temp);
        printArray(FFT(src));
    }
}

class Complex {
    double re;
    double im;
    
    public Complex(double real, double imag) {
        re = real;
        im = imag;
    }

    public Complex add(Complex b) {
        Complex a = this;
        double real = a.re + b.re;
        double imag = a.im + b.im;
        return new Complex(real, imag);
    }

    public Complex sub(Complex b) {
        Complex a = this;
        double real = a.re - b.re;
        double imag = a.im - b.im;
        return new Complex(real, imag);
    }

    public Complex mul(Complex b) {
        Complex a = this;
        double real = a.re * b.re - a.im * b.im;
        double imag = a.re * b.im + a.im * b.re;
        return new Complex(real, imag);
    }

    public String toString() {
        if (im == 0) {
            return String.format("[%.2f]", re);
        }
        if (re == 0) {
            return String.format("[%.2f i]", im);
        }
        if (im < 0) {
            return String.format("[%.2f - %.2f i]", re, (-1) * im);
        }
        return String.format("[%.2f + %.2f i]", re, im);
    }
}
```
<br/>
=============
<br/>
**실행 결과**<br/>
<br/>
다음 코드의 실행 결과이다.<br/>
DFT와 FFT의 결과가 동일함을 확인할 수 있다.<br/>
