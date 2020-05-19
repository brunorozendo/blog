---
title: "Calcular π (pi)"
description: "algoritmo em java para calcular pi"
author:
  name: "Bruno Rozendo"
date: 2016-11-12
tags:
- matemática
- algoritmos
- pi
comments: true
imageCredit: "<a href='http://www.freepik.com'>Designed by Freepik</a>"
image: "/images/posts/calcular-pi.jpg" 
math: true
---


Seja a fórmula matemática para calcular Pi (<span class="MJXc-TeX-math-I">&Pi;&#x0007C;&pi;</span>)

{{< math >}}
<math xmlns="http://www.w3.org/1998/Math/MathML" display="block">
  <mi>&pi;</mi>
   <mo>=</mo>
   <mn>4</mn>
  <mfenced separators="">
  <mn>1</mn>
  <mo>&minus;</mo>
  <mfrac>
     <mn>1</mn>
     <mn>3</mn>
  </mfrac>
  <mo>&plus;</mo>
  <mfrac>
     <mn>1</mn>
     <mn>5</mn>
  </mfrac>
  <mo>&minus;</mo>
  <mfrac>
     <mn>1</mn>
     <mn>7</mn>
  </mfrac>
  <mo>&plus;</mo>
  <mfrac>
     <mn>1</mn>
     <mn>9</mn>
  </mfrac>
  <mi>&ctdot;</mi>
  </mfenced>
</math>
{{< /math >}}

Fazendo algumas operações algebricas para que a fórmula fique mais fácil de trabalha no algoritmo:

{{< math >}}
<math xmlns="http://www.w3.org/1998/Math/MathML" display="block">
<mi>&pi;</mi>
 <mo>=</mo>
<mfrac>
   <mn>4</mn>
   <mn>1</mn>
</mfrac>
<mo>&minus;</mo>
<mfrac>
   <mn>4</mn>
   <mn>3</mn>
</mfrac>
<mo>&plus;</mo>
<mfrac>
   <mn>4</mn>
   <mn>5</mn>
</mfrac>
<mo>&minus;</mo>
<mfrac>
   <mn>4</mn>
   <mn>7</mn>
</mfrac>
<mo>&plus;</mo>
<mfrac>
   <mn>4</mn>
   <mn>9</mn>
</mfrac>
<mi>&ctdot;</mi>
</math>
{{< /math >}}

o algoritmo:


{{< highlight java >}}
import java.math.BigDecimal;
import java.math.MathContext;
import java.math.RoundingMode;

public class Pi{
  public Pi(){
    int precisao = 15000;
    MathContext mx                   = new MathContext(precisao, RoundingMode.HALF_EVEN);

    BigDecimal quatro                = new BigDecimal( 4, mx);
    BigDecimal pi                    = new BigDecimal( 0, mx);
    BigDecimal sinal                 = new BigDecimal( 1, mx);
    BigDecimal incrementoDenominador = new BigDecimal( 2, mx);
    BigDecimal denominador           = new BigDecimal( 1, mx);

    for (int i = 0 ; i < precisao ; i++) {
      BigDecimal tmp  = quatro.divide(denominador, mx);
      tmp = tmp.multiply(sinal, mx);
      pi = pi.add(tmp, mx);

      sinal = sinal.negate();
      denominador = denominador.add(incrementoDenominador);
    }
    System.out.print("\u03c0: ");
    System.out.println(pi);
  }  
  public static void main(String[] args) {
    new Pi();
  }
}
{{< /highlight >}}

