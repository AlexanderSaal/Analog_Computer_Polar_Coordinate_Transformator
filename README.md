# Analog_Computer_Polar_Coordinate_Transformator $(r,\varphi) \rightarrow (x,y)$
---

Analog computer module for transforming polar coordinates $(r,\varphi)$ into cartesian coordinates $(x,y)$, by using the relationship:

$$
\begin{align}
  x & = r \cdot \cos(\varphi) \\
  y & = r \cdot \sin(\varphi)
\end{align}
$$

Since the direct calculatino of the function _sine_ and _cosine_ is complex to implement in an electrical circuit, I realy here on the obsolete IC *AD639* of [Analog Devices](https://www.analog.com/en/products/ad639.html#part-details).\
The purpose of this project is to learn more about analog circuits and implementations. The choice to base this circuit on an older obsolete IC is made out of admiration rather than efficiency. The reader might be able to implement a more modern and cost-efficient circuit in a digital-analog hybrid circuit. I plan to explore this option in a future project.

## Schematic

The incoming $\varphi$-signal is scaled down from the machine unit voltage of $10V$ to the scale of $50^\circ/V$, of the AD639, which is $7.2V$. Additionally, the OP-amp is an isolation of the input, to reduce the risk of damaging the AD639.\
The $r$-signal is buffered by an ideal diode circuit to get the absolute value of the signal. This ensures operation, even if the user inputs a negative signal.\
The output signal is also buffered by the same OP-amp to isolate the output of the AD639. This will protect the chip from any accidents, when wiring up the output incorrectly.\
Lastly, the error state of the AD639 is displayed by an LED.


![schematic](schematic/schematic-1.png "Circuit implementation of the polar coordinate transformer")


## Reverse transformation $(x,y) \rightarrow (r,\varphi)$

The reverse is possible: *$(x,y) \rightarrow (r,\varphi)$*\
although more difficult and arguably used even more rarely.\
For completeness sake and as a reminder, I will mention my suggestions for the circuit implementation.\
In general the math is given by:

$$
\begin{align}
  r(x,y) & = \sqrt{x^2 + y^2} \\
  \varphi(x,y) & = r \cdot atan2(x,y) =r \cdot \begin{cases}
  \arctan\left(\frac{y}{x}\right)       & \text{if } x > 0\\
  \arctan\left(\frac{y}{x}\right) + \pi & \text{if } x < 0 \text{ and } y \ge 0\\
  \arctan\left(\frac{y}{x}\right) - \pi & \text{if } x < 0 \text{ and } y < 0\\
  \frac{\pi}{2}                         & \text{if } x = 0 \text{ and } y > 0\\
  -\frac{\pi}{2}                         & \text{if } x = 0 \text{ and } y < 0\\
  \text{undefined}                      & \text{if } x = 0 \text{ and } y = 0.
\end{cases}
\end{align}
$$

which means, that we have to many comperators in the circuit to differentiate between 6 different cases in the input $(x,y)$. \
To save on components and to simplify the math, we can look at another option. If the value of $r$ is known at the time of calculation and if the value is $r \neq 0$, then we can use another trigonometric relationship and rewrite $\varphi(x,y)$ into $\varphi(x,r)$:

$$
\begin{equation}
  \varphi(x,y) = \begin{cases}
   \arccos\left(\frac{x}{r}\right) & \text{if } y \ge 0 \\
  -\arccos\left(\frac{x}{r}\right) & \text{if } y < 0 \\
   \text{undefined}                & \text{if } r = 0.
\end{cases}
\end{equation}
$$

reducing the cases to differentiate to one.\

To be compatible with the previous circuit, I would take care that the output also scales with $50^\circ/V$.

## License

This work is published under the [CERN Open Hardware Licence Version 2 - Strongly Reciprocal](LICENSE)
