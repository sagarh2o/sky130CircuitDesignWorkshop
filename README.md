#
# Sky130 Circuit Design Workshop

This workshop aim towards the basic understanding of a semiconductor device namely MOSFET, which is widely used as a switching device and also as an amplifier. Along with the MOS device the workshop also emphasis on CMOS inverter and its robustness. It also included various labs that provided hands-on experience on ngspice via a web-based Linux virtual machine. The lab involved writing various spice decks and their simulations. The content of the workshop was taught over 5 days.

# Contents

[Sky130 Circuit Design Workshop 1](#_Toc65542929)

[Important GitHub Repositories 1](#_Toc65542930)

[Day 1 2](#_Toc65542931)

[Day 2 4](#_Toc65542934)

[Day 3 6](#_Toc65542937)

[Day 4 9](#_Toc65542940)

[Day 5 11](#_Toc65542943)

[Conclusion 12](#_Toc65542946)

[Acknowledgment 12](#_Toc65542947)

[References 12](#_Toc65542948)

# Important GitHub Repositories

- [https://github.com/kunalg123/vsdflow.git](https://github.com/kunalg123/vsdflow.git)
- [https://github.com/nickson-jose/openlane\_build\_script.git](https://github.com/nickson-jose/openlane_build_script.git)
- [https://github.com/kunalg123/sky130CircuitDesignWorkshop.git](https://github.com/kunalg123/sky130CircuitDesignWorkshop.git)

# Day 1

## Theoretical

Day1 included an introduction to circuit design and spice. On this day a fair emphasis was put on so as to why do we need SPICE. Knowledge was shredded on the basic element of Circuit design i.e nMOS. Various concepts were taught like :

- nMOS structure
- Strong Inversion and threshold voltage
- Body Effect
- Pinch-off
- Various modes of operation
- Current equations w.r.t the MOSFET&#39;s mode of operation

## Lab Session

On the first day of the workshop, we were introduced to the cloud-based Linux virtual machine. We were guided so as to how we need to proceed with the lab in the upcoming days.

The lab started with the cloning of the git repo from kunalg123&#39;s Github repository. Along with this, we were taught about various terminal command lines.

To clone the files simply open the terminal and type

git clone [https://github.com/kunalg123/sky130CircuitDesignWorkshop.git](https://github.com/kunalg123/sky130CircuitDesignWorkshop.git)

This downloaded all the required files that are needed to complete the 5-day workshop. Which included various:

- spice decks
- model files
- technological files
- cells

A directory is made as shown below

![1](https://user-images.githubusercontent.com/20883889/109559307-7fd7d600-7b00-11eb-8ca0-71c2c47d283c.jpg)

_Figure 1 Directory after git clone_

We were taught about SPICE syntax. To simulate a given circuit 3 things are required:

1. Spice Netlist- That describes a given circuit
2. Spice model parameters- This is a .mod file that contains information about all the parameters of the device which are provided by the foundry
3. Spice simulation commands- To mathematically predict the behavior of the electronic circuit

![2](https://user-images.githubusercontent.com/20883889/109559500-bf062700-7b00-11eb-8541-1d5b7ad90812.jpg)

Wrote SPICE deck for a nMOS circuit. Leafpad was used to write/edit the spice files.

![3](https://user-images.githubusercontent.com/20883889/109559547-d0e7ca00-7b00-11eb-89c8-4a3a700b84dc.jpg)

_Figure 2 circuit nodes shown in Nmos circuit_

\*Model Description

.param temp=27

\*Including sky130 library files

.lib &quot;sky130\_fd\_pr/models/sky130.lib.spice&quot; tt

\*Netlist Description

XM1 Vdd n1 0 0 sky130\_fd\_pr\_\_nfet\_01v8 w=5 l=2

R1 n1 in 55

Vdd vdd 0 1.8V

Vin in 0 1.8V

\*simulation commands

.op

.dc Vdd 0 1.8 0.1 Vin 0 1.8 0.2

.control

run

display

setplot dc1

.endc

.end

The above spice deck was already there in the directory that was cloned from the repository by the name of &#39;day1\_nfet\_idvds\_L2\_W5.spice&#39;. To simulate we just need to change the directory to the working directory in which the file is saved. In my case, it was Sagarsaini2412@sagarsaini2412-VirtualBox:~/sky130CircuitDesignWorkshop/design$

- To simulate type in the following commands:

ngspice day1\_nfet\_idvds\_L2\_W5.spice

ngspice-\&gt; plot -vdd#branch

after these commands, you will get the drain characteristics of the above MOSFET

![4](https://user-images.githubusercontent.com/20883889/109559618-e5c45d80-7b00-11eb-9e4c-f8e14ef8ff91.jpg)

_Figure 3 Mosfet drain characteristics with Vdd, vin= 1.8v, w=5um, and l=2um (a long channel device)._

In the above graph, all three modes of operation the cut-off, linear, and saturation regions are visible.

# Day 2

## Theoretical

Day 2 was dedicated to lower node devices and basic knowledge of CMOS inverter. Lower node devices are the ones that have channel lengths less than 0.25 µm. These are also called short-channel mosfets. In these types of devices along with 3 modes of operation, there is an extra mode of operation known as velocity saturation mode. This occurs because at higher electric field the velocity of the charge carrier becomes constant due to the scattering effect.

There are two models one for a long-channel device and the other one is for a short-channel device. So, we have to make a model that is common for both and that came up to be

![5](https://user-images.githubusercontent.com/20883889/109559679-f8d72d80-7b00-11eb-8407-02fae38a27af.jpg)

Apart from this a lot of theoretical concepts were taught like:

- Basic pmos working
- Mosfet as a switch
- Cmos working- pull up and pull down network
- Pmos nmos drain current vs drain voltage
- Pmos nmos Vout vs Vin
- Merging of pmos and nmos load curves to obtain VTC i.e Voltage transfer characteristics

Now as we know that any device is known by its Vout vs Vin characteristics but not by its intermediate currents and voltages. So it&#39;s important to convert these pmos and nmos drain current vs drain voltage graph into Vout vs Vin.

## Lab Session

In day2 lab, simulation of a short channel device was done and then its characteristics were compared with that of a long channel device while keeping the W/L ratio constant.

- Commands to simulate drain characteristics for a short channel device, the commands are:

ngspice day2\_nfet\_idvds\_L015\_W039

plot -vdd#branchh

![6](https://user-images.githubusercontent.com/20883889/109559722-0a203a00-7b01-11eb-83d8-450e88f43785.jpg)

_Figure 4 Drain characteristics of a (a) long channel device (b) short channel device_

From day1 lab, we obtained drain characteristics for a long channel device, and in that the drain current increases quadratically with the increase in Vds. whereas from day2 lab, we obtain drain characteristics for a short channel device which increases linearly. This is due to velocity saturation.

- Commands to simulate transfer characteristics:

1. For long channel device - open Leafpad and change the W/L parameter, W= 5µm, and L=2µm and then simulate. Commands for that are:

leafpad day2\_nfet\_idvgs\_L015\_W039

ngspice day2\_nfet\_idvgs\_L015\_W039

plot -vdd#branchh

1. For short channel device - open Leafpad and change the W/L parameter, W= 0.39µm, and L=0.15µm and then simulate. Commands for that are:

leafpad day2\_nfet\_idvgs\_L015\_W039

ngspice day2\_nfet\_idvgs\_L015\_W039

plot -vdd#branchh

In the transfer characteristics shown below, one can see that the drain current quadratically increases with Vgs. Whereas, in the case of a short channel device the drain current in the start increases quadratically but later on it increases linearly.

![7](https://user-images.githubusercontent.com/20883889/109559769-1d330a00-7b01-11eb-91c3-66abed22463b.jpg)

_Figure 5 transfer characteristics of (a) long channel device (b) shot channel device_

# Day 3

## Theoretical

Theoretical concepts that were covered on this day included:

- Spice deck for cmos inverter to obtain VTC curve
- Various modes of cmos inverter (5 in total)

- Deriving analytical expression for Vm
- Relationship between Vm and (W/L) ratio
- Dynamic behavior of cmos inverter
- Rise delay and fall delay
- Spice deck for calculation of rise delay and fall delay

![ds](https://user-images.githubusercontent.com/20883889/109559879-4489d700-7b01-11eb-9c82-f87dfce9ed33.jpg)

_Figure 6 Cmos inverter circuit_

From the above circuit, one can write a spice netlist for a cmos inverter.

Switching Threshold, Vm: It is defined as a point where Vin=Vout and both pmos and nmos operate in the saturation region. It can be identified from the VTC curve, it is a point where a Vout=Vin line intersects with the VTC curve.

![dsd](https://user-images.githubusercontent.com/20883889/109559929-52d7f300-7b01-11eb-80bb-5456bba68e62.jpg)

Rise delay: The delay in propagation when the output rises from low to high when the input switches from high to low and it is generally calculated at 50% of the input/output value.

Fall delay: The delay in propagation when the output falls from high to low when the input switches from low to high and it is also calculated at 50% of the input/output value.

## Lab Session

On day3, the lab session was divided into 2 parts.

Firstly, simulation of the cmos inverter circuit was done with a varying pmos width (wp) while keeping Lp=ln=0.15u and wn=0.36u constant to obtain the VTC curve. From that VTC curve, the switching threshold was obtained from the intersection of VTC curve and Vout=Vin line.

- Commands to simulate cmos inverter circuit to obtain VTC curve:

leafpad day3\_inv\_vtc\_Wp084\_Wn036

ngspice day3\_inv\_vtc\_Wp084\_Wn036

plot out vs in

the leafpad command is to open the editor and change the values of Wp

![8](https://user-images.githubusercontent.com/20883889/109560100-7bf88380-7b01-11eb-836f-138d4752a34c.jpg)

_Figure 7 VTC curve showing values of Vm for (a) wp=0.42u (b) wp=1.68u_

From the above graphs, one can see that with the increase in the wp from 0.42u to 1.68u the switching threshold shift from left to right, in the above case from 0.861V to 0.906V.

Secondly, Simulation of the cmos inverter circuit is done to obtain the rise delay and the fall delay. Again different rise and fall delays are measured with a varying width of pmos i.e Wp. To measure the delays, transient simulation commands should be added to the spice deck.

- Commands to simulate cmos inverter circuit and obtain rise and the fall delay:

leafpad day3\_inv\_tran\_Wp084\_Wn036

ngspice day3\_inv\_tran\_Wp084\_Wn036

plot out vs time in

![9](https://user-images.githubusercontent.com/20883889/109560108-7d29b080-7b01-11eb-9644-765acd82313a.jpg)

_Figure 8 graph showing rise and fall delay for (a) Wp=0.42 (b) Wp=1.68_

From the above graph, we can see that as we increase the (Wp/Lp)/(Wn/Ln) ratio, the rise delay keeps on decreasing. More simulations were done for Wp=0.84u,1u, 1.26u, and corresponding Vm, rise, and fall delay were noted.

| **Wp** | **Wn** | **Ln=Lp** | **Rise delay** | **Fall delay** | **Vm** |
| --- | --- | --- | --- | --- | --- |
| 0.42u | 0.36u | 0.15u | 0.615ns | 0.268ns | 0.861V |
| 0.84u | 0.36u | 0.15u | 0.330ns | 0.480ns | 0.876V |
| 1.00u | 0.36u | 0.15u | 0.282ns | 0.285ns | 0.880V |
| 1.26u | 0.36u | 0.15u | 0.235ns | 0.285ns | 0.881V |
| 1.68u | 0.36u | 0.15u | 0.179ns | 0.298ns | 0.903V |

_Figure 9 Table showing Vm, Rise and fall delay with a change in Wp_

From the above table, we can infer that:

- with the increase in (Wp/Lp)/(Wn/Ln) ratio, there is an increase in the switching threshold, Vm but the change in Vm w.r.t change in the ratio is not so much and is of the range of 50mV and thus shows the robustness of the cmos inverter.
- With the increase in (Wp/Lp)/(Wn/Ln), there is a decrease in rise delay. From the above table if we notice that around the ratio of 2 i.e (Wp/Lp)~2\*(Wn/Ln), the slew is the same which is a basic requirement of a cell (especially inverter). These types are used in clock tree synthesis, CTS. Whereas, the cmos with (Wp/Lp)~5\*(Wn/Ln) provides the least amount of rise delay which are generally used in data paths.

# Day 4

##

## Theoretical

Day4 was dedicated to the understanding of the Noise margin concept. In lower node devices one frequent problem is crosstalk noise and glitches. A good device is a one that can neglect all those noises and can still operate to do the task it was meant to. Noise margin is the amount of noise that can be added to the cmos circuit without altering its operation. As we know that cmos can be used in digital design. Thus in this case noise margin should such that a logic &#39;1&#39; should be recognized as logic &#39;1&#39; only, not as logic &#39;0&#39; and vice versa. If by chance the voltage bump lies between the undefined region then that is an unsafe glitch and needs to be fixed.

![10](https://user-images.githubusercontent.com/20883889/109560111-7d29b080-7b01-11eb-934c-f68e792bf5a6.jpg)

_Figure 10 noise margin in VTC graph_

To calculate noise margins, two points are taken which intersect with the VTC curve and -1 slope lines. Corresponding output and input voltage levels are noted which will be used to calculate the noise margins. Below is a map showing different voltages and two noise margins. One is Noise margin high which is used to differentiate logic &#39;1&#39; and the other is Noise margin low to differentiate logic &#39;0&#39;.

![11](https://user-images.githubusercontent.com/20883889/109560114-7dc24700-7b01-11eb-9eb9-140378f79267.jpg)

_Figure 11 Noise margin voltage map_

Noise margin high, NMH = VOH – VIH

Noise margin low, NML = VIL – VOH

##

## Lab Session

On day4 lab, we simulated the cmos inverter circuit to calculate the noise margins with varying (Wp/Lp)/(Wn/Ln) ratio.

- Commands to simulate the cmos inverter circuit and obtain Noise margins:

leafpad day4\_inv\_noisemargin\_wp1\_wn036

ngspice day4\_inv\_noisemargin\_wp1\_wn036

plot out vs in

the leafpad command is to open the editor and change the values of Wp.

![12](https://user-images.githubusercontent.com/20883889/109560117-7dc24700-7b01-11eb-9b6d-6218219f2e38.jpg)

_Figure 12 Noise margins shown in VTC graph (a) Wp=0.42u (b) Wp=1.68u_

More simulations were done for Wp=0.84u,1u, 1.26u, and corresponding noise margins were noted.

| **Wp** | **Wn** | **Ln=Lp** | **NM**** H **|** NM ****L** |
| --- | --- | --- | --- | --- |
| 0.42u | 0.36u | 0.15u | 0.30V | 0.30V |
| 0.84u | 0.36u | 0.15u | 0.35V | 0.30V |
| 1.00u | 0.36u | 0.15u | 0.40V | 0.30V |
| 1.26u | 0.36u | 0.15u | 0.42V | 0.27V |
| 1.68u | 0.36u | 0.15u | 0.42V | 0.27V |

_Figure 13 Table showing Noise margins With varying (Wp/Lp)/(Wn/Ln) ratio_

From the above table, one can infer that there is not much variation with varying (Wp/Lp)/(Wn/Ln) ratio. This shows the robustness of cmos as not much deviation is shown in the operation with a deviation of device parameter. If you want to vary the noise margin one can increase the size of the pmos and NMH will increase but not by much amount. This happens because pmos is the one that helps to charge the capacitance. If you increase the size of pmos the resistance path also decreases. That is why it can hold the charges in the capacitance for a longer period.

# Day 5

##

## Theoretical

On day5 following concepts were covered:

- Cmos static behavior w.r.t power supply variation
- Writing smart simulation commands in spice deck for supply variation
- Cmos static behavior w.r.t device variation (W/L) due to etching
- Cmos static behavior w.r.t device variation (tox) due to oxidation
- Writing smart simulation commands in spice deck for device variation

After fabrication, it&#39;s not like that your device will be manufactured perfectly with no imperfection. Generally, device parameters vary due to various processes that the device goes under during fabrication for e.g the W/L ratio vary due to etching and tOX vary due to oxidation.

![13](https://user-images.githubusercontent.com/20883889/109560121-7e5add80-7b01-11eb-9c45-e4ccf83451b7.jpg)

_Figure 14 Device variation due to (a) etching (b) oxidation_

## Lab session

Day5 lab is divided into two parts:

1. Static behavior of cmos is simulated with varying supply voltage

To do so simply type in terminal:

ngspice day5\_inv\_supplyvariation\_Wp1\_Wn036

![14](https://user-images.githubusercontent.com/20883889/109560123-7e5add80-7b01-11eb-857b-ef3eba4dc65c.jpg)

_Figure 15 various VTC graph shown with varying supply voltage_

From the above graph, we can infer that with a decrease in supply voltage the VTC graph shrinks. While using lower supply voltage we can get a higher gain (close to 50% improvement) along with low energy consumption. For e.g, if we take 0.5 supply voltage and compare it with 2.5 supply voltage, then there is a significant energy reduction (almost 90%). But there is a disadvantage too i.e. with 0.5 supply voltage the rise time becomes very high and thus the circuit becomes slow. It becomes so slow that the capacitor is not able to charge to its maximum value. Thus there is a significant reduction in performance.

1. Static behavior of cmos is simulated with the varying device parameter

To do so simply type in terminal:

ngspice day5\_inv\_devicevariation\_wp7\_wn042

plot out vs in

![15](https://user-images.githubusercontent.com/20883889/109560125-7ef37400-7b01-11eb-8b80-621da9f2cfbf.jpg)

_Figure 16 Device variation (a) Wp=1.68u, Wn=0.36u (b) Wp=0.42u, Wn=1.68u_

In the above simulation, the width of pmos and nmos are varied from Wp=1.68u, Wn=0.36u to Wp=0.42u, Wn=1.68u, and from the graphs, we can see that there is not much deviation in the voltage transfer characteristics of the cmos. The deviation is about 0.2V, which is acceptable.

# Conclusion

In the 5-day workshop, I learned about MOSFET its characteristics, cmos and its characteristics, cmos application, cmos robustness w.r.t to switching threshold, noise margin, supply voltage, and device parameter. Apart from this, I learned about Linux operating system and working on ngspice.

Overall it was a great learning experience.

# Acknowledgment

Kunal Ghosh, Co-founder (VSD Corp. Pvt. Ltd)

# References

| [1] | K. Ghosh, &quot;Udemy,&quot; [Online]. Available: https://www.udemy.com/course/vlsi-academy-circuit-design/. [Accessed Feb 2021]. |
| --- | --- |
| [2] | K. Ghosh, &quot;Udemy,&quot; [Online]. Available: https://www.udemy.com/course/vlsi-academy-circuit-design-part2/. [Accessed Feb 2021]. |
| [3] | &quot;vlsisystemdesign,&quot; [Online]. Available: https://www.vlsisystemdesign.com/. [Accessed feb 2021]. |
| [4] | K. Ghosh, &quot;Udemy,&quot; [Online]. Available: https://www.udemy.com/course/vsd-a-complete-guide-to-install-open-source-eda-tools/. [Accessed Feb 2021]. |
| [5] | K. G. Nickson Jose, &quot;Udemy,&quot; [Online]. Available: https://www.udemy.com/course/vsd-a-complete-guide-to-install-openlane-and-sky130nm-pdk/. [Accessed Feb 2021]. |
